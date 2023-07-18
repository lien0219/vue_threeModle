<template>
  <div id="container"></div>
</template>

<script>
import * as THREE from "three";
// 导入控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
export default {
  name: "LoadingModel2",
  data() {
    return {
      // img: require("../../dist/texture/sky.jpg"),
    };
  },
  mounted() {
    // 初始化场景
    const scene = new THREE.Scene();
    // 初始化相机
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      2000
    ); ///透视相机
    //相机位置
    camera.position.set(-50, 50, 130);
    // 更新摄像机宽高比
    camera.aspect = window.innerWidth / window.innerHeight;
    // 更新摄像头投影矩阵
    camera.updateProjectionMatrix();
    scene.add(camera);

    // 初始化渲染器
    const renderer = new THREE.WebGL1Renderer({
      // 设置抗锯齿
      antialias: true,
    });
    renderer.outputEncoding = THREE.sRGBEncoding;

    // 设置渲染器宽高
    renderer.setSize(window.innerWidth, window.innerHeight);

    // 将渲染器添加到页面
    document.getElementById("container").appendChild(renderer.domElement);

    // 实例化控制器
    const controls = new OrbitControls(camera, renderer.domElement);
    console.log(controls);

    function render() {
      // 渲染场景
      renderer.render(scene, camera);
      // 引擎自动更新渲染器
      requestAnimationFrame(render);
    }
    render();

    // 添加平面
    const planeGeometry = new THREE.PlaneGeometry(100, 100);
    const planeMaterial = new THREE.MeshBasicMaterial({
      color: 0xffffff,
    });
    const plane = new THREE.Mesh(planeGeometry, planeMaterial);
    scene.add(plane);

    // 创建球
    // const skyGeometry = new THREE.SphereGeometry(1000, 60, 60);
    // const skyMaterial = new THREE.MeshBasicMaterial({
    //   map: new THREE.TextureLoader().load("./imgs/sky.jpg"),
    // });
    // const sky = new THREE.Mesh(skyGeometry, skyMaterial);
    // scene.add(sky);
  },
};
</script>

<style></style>
