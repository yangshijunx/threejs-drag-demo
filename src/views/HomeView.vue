<!-- <template>
  <div ref="canvasContainer" style="width: 500px; height: 500px"></div>
</template>

<script>
import * as THREE from "three";

export default {
  mounted() {
    this.initThree();
  },
  methods: {
    initThree() {
      // 创建场景
      const scene = new THREE.Scene();

      // 创建相机
      const camera = new THREE.PerspectiveCamera(
        75,
        this.$refs.canvasContainer.offsetWidth /
          this.$refs.canvasContainer.offsetHeight,
        0.1,
        1000
      );
      camera.position.z = 5;

      // 创建渲染器
      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(
        this.$refs.canvasContainer.offsetWidth,
        this.$refs.canvasContainer.offsetHeight
      );
      this.$refs.canvasContainer.appendChild(renderer.domElement);

      // 创建红色材质
      const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });

      // 创建四个圆形
      for (let i = 0; i < 4; i++) {
        const geometry = new THREE.CircleGeometry(0.15, 32);
        const circle = new THREE.Mesh(geometry, material);
        circle.position.x = (i % 2) * 2 - 0.5; // 根据索引位置计算x坐标
        circle.position.y = Math.floor(i / 2) * 2 - 0.5; // 根据索引位置计算y坐标
        scene.add(circle);
      }

      // 渲染场景
      const animate = () => {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
      };
      animate();
    },
  },
};
</script>

<style>
canvas {
  display: block;
}
</style> -->
<!-- <template>
  <div ref="canvasContainer" style="width: 500px; height: 500px"></div>
</template>

<script>
import * as THREE from "three";
import { DragControls } from "three/examples/jsm/controls/DragControls.js";

export default {
  mounted() {
    this.initThree();
  },
  methods: {
    initThree() {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(
        75,
        this.$refs.canvasContainer.offsetWidth /
          this.$refs.canvasContainer.offsetHeight,
        0.1,
        1000
      );
      camera.position.z = 5;

      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(
        this.$refs.canvasContainer.offsetWidth,
        this.$refs.canvasContainer.offsetHeight
      );
      this.$refs.canvasContainer.appendChild(renderer.domElement);

      const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });

      // 创建四个圆形并添加到场景中
      const circles = [];
      for (let i = 0; i < 4; i++) {
        const geometry = new THREE.CircleGeometry(0.15, 32);
        const circle = new THREE.Mesh(geometry, material);
        circle.position.x = (i % 2) * 2 - 0.5;
        circle.position.y = Math.floor(i / 2) * 2 - 0.5;
        circle.cursor = "pointer";
        scene.add(circle);
        circles.push(circle);
      }

      // 创建DragControls实例并传入要控制的对象和相机
      const dragControls = new DragControls(
        circles,
        camera,
        renderer.domElement
      );

      const animate = () => {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
      };
      animate();
    },
  },
};
</script>

<style>
canvas {
  display: block;
}
</style> -->
<template>
  <div ref="canvasContainer" style="width: 500px; height: 500px"></div>
</template>

<script>
import * as THREE from "three";
import { DragControls } from "three/examples/jsm/controls/DragControls.js";

export default {
  mounted() {
    this.initThree();
    // this.loadSavedPositions(); // 加载之前保存的位置
  },
  methods: {
    // loadSavedPositions() {
    //   console.log("执行查找位置");
    //   // 检查localStorage中是否存储了位置
    //   for (let i = 0; i < 4; i++) {
    //     const savedPosition = localStorage.getItem(`circle${i + 1}`);
    //     if (savedPosition) {
    //       const { x, y } = JSON.parse(savedPosition);
    //       this.moveCircle(i, parseFloat(x), parseFloat(y));
    //     }
    //   }
    // },
    // moveCircle(index, x, y) {
    //   const circles = this.$refs.canvasContainer.children[0].children;
    //   circles[index].position.x = x;
    //   circles[index].position.y = y;
    // },
    initThree() {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(
        75,
        this.$refs.canvasContainer.offsetWidth /
          this.$refs.canvasContainer.offsetHeight,
        0.1,
        1000
      );
      camera.position.z = 5;

      const renderer = new THREE.WebGLRenderer();
      renderer.setSize(
        this.$refs.canvasContainer.offsetWidth,
        this.$refs.canvasContainer.offsetHeight
      );
      this.$refs.canvasContainer.appendChild(renderer.domElement);

      const material = new THREE.MeshBasicMaterial({ color: 0xff0000 });

      // 创建四个圆形并添加到场景中
      const circles = [];
      for (let i = 0; i < 4; i++) {
        const geometry = new THREE.CircleGeometry(0.15, 32);
        const circle = new THREE.Mesh(geometry, material);
        const savedPosition = localStorage.getItem(`circle${i + 1}`);
        if (savedPosition) {
          //   console.log("记录位置", savedPosition);
          let position = JSON.parse(savedPosition);
          circle.position.x = position.x;
          circle.position.y = position.y;
          console.log("走哪里？", position.x, position.y);
        } else {
          circle.position.x = (i % 2) * 2 - 0.5;
          circle.position.y = Math.floor(i / 2) * 2 - 0.5;
        }
        circle.cursor = "pointer";
        circle.onlyid = i + 1;
        scene.add(circle);
        circles.push(circle);
      }

      // 创建DragControls实例并传入要控制的对象和相机
      const dragControls = new DragControls(
        circles,
        camera,
        renderer.domElement
      );

      // 监听拖拽事件
      dragControls.addEventListener("drag", (item) => {
        let onlyid = item.object.onlyid;
        console.log("这是哪一个", onlyid);
        circles.forEach((circle, index) => {
          if (circle.onlyid === onlyid) {
            // 打印坐标
            console.log(
              `Circle ${onlyid} Position: ${circle.position.x.toFixed(
                2
              )}, ${circle.position.y.toFixed(2)}`
            );
            // 存储坐标到localStorage
            localStorage.setItem(
              `circle${onlyid}`,
              JSON.stringify({ x: circle.position.x, y: circle.position.y })
            );
          }
        });
      });

      const animate = () => {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
      };
      animate();
    },
  },
};
</script>

<style>
canvas {
  display: block;
}
</style>
