<template>
  <!-- vr看房 -->
  <div ref="container"></div>
</template>

<script>
import * as THREE from "three";
// 引入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 加载器
import { RGBELoader } from "three/examples/jsm/loaders/RGBELoader";
export default {
  name: "VrHouse",
  mounted() {
    //初始化场景
    const scene = new THREE.Scene();

    // 初始化相机
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    // 设置相机位置
    // camera.getWorldPosition.z = 1;
    camera.position.set(0, 0, 5);

    // 初始化渲染器
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);

    const render = () => {
      renderer.render(scene, camera);
      requestAnimationFrame(render);
    };

    // 添加立方体
    // const geometry = new THREE.BoxGeometry(10, 10, 10);
    // // const material = new THREE.MeshBasicMaterial({
    // //   color: "green", //0xff0000设置材质颜色为红色
    // // });
    // // const cube = new THREE.Mesh(geometry, material);
    // // scene.add(cube);

    // // 4_b, 在创建的立方体上贴图
    // var arr = ["4_l", "4_r", "4_u", "4_d", "4_b", "4_f"];
    // var boxMaterials = [];
    // arr.forEach((item) => {
    //   // 纹理加载
    //   let texture = new THREE.TextureLoader().load(`./imgs/living/${item}.jpg`);
    //   //  创建材质
    //   boxMaterials.push(new THREE.MeshBasicMaterial({ map: texture }));
    //   //   if (item === "4_u" || item === "4_d") {
    //   //     // 如果是上下面，旋转
    //   //     texture.rotation = Math.PI;
    //   //     texture.center = new THREE.Vector2(0.5, 0.5);
    //   //     boxMaterials.push(new THREE.MeshBasicMaterial({ map: texture }));
    //   //   } else {
    //   //     boxMaterials.push(new THREE.MeshBasicMaterial({ map: texture }));
    //   //   }
    // });
    // const cube = new THREE.Mesh(geometry, boxMaterials);
    // cube.geometry.scale(1, 1, -1); // 调节z 轴看到内部
    // scene.add(cube);

    // 添加球
    const geometry = new THREE.SphereGeometry(5, 32, 32);
    const loader = new RGBELoader(); // 加载器
    loader.load("./imgs/hdr/Living.hdr", (texture) => {
      const material = new THREE.MeshBasicMaterial({ map: texture });
      const sphere = new THREE.Mesh(geometry, material);
      sphere.geometry.scale(1, 1, -1);
      scene.add(sphere);
    });

    // 添加轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;

    this.$refs.container.appendChild(renderer.domElement);
    render();
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
.container {
  width: 100vw;
  height: 100vh;
  background: white;
}
</style>
