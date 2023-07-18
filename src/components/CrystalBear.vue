<template>
  <div ref="CrystalBear" class="CrystalBear"></div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
// 导入控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
export default {
  name: "CrystalBear",
  data() {
    return {};
  },
  mounted() {
    // 初始化场景
    const scene = new THREE.Scene();
    // 初始化相机
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    // 初始化相机位置
    camera.position.set(1.5, 1, 2.5);
    camera.aspect = window.innerWidth / window.innerHeight;
    // 更新相机投影矩阵
    camera.updateProjectionMatrix();
    // 加载背景纹理
    const loader = new THREE.TextureLoader();
    const bgTexture = loader.load("/assets/hai.gif");
    bgTexture.mapping = THREE.EquirectangularRefractionMapping;

    scene.background = bgTexture;
    scene.environment = bgTexture;

    // 添加环境光
    const ambient = new THREE.AmbientLight(0xffffff, 0.5);
    scene.add(ambient);

    // 加载模型
    const gltfLoader = new GLTFLoader();
    gltfLoader.load("/assets/weixing/scene.gltf", (gltf) => {
      //   const model = gltf.scene;
      const model = gltf.scene.children[0];

      //   自选配置，模型处理好可不写
      model.material = new THREE.MeshPhongMaterial({
        color: "pink",
        flatShading: true,
        envMap: bgTexture, //环境纹理
        refractionRatio: 0.7, //折射
        reflectivity: 0.99, //反射
        // opacity: 0.5,
      });

      model.scale.set(0.1, 0.1, 0.1);
      scene.add(model);
    });

    // 初始化渲染器
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    // 设置渲染器容器
    renderer.setSize(window.innerWidth, window.innerHeight);
    // 监听屏幕大小变化
    window.addEventListener("resize", () => {
      renderer.setSize(window.innerWidth, window.innerHeight);
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
    });
    // 将渲染器添加到页面
    this.$refs.CrystalBear.appendChild(renderer.domElement);
    // 添加控制器
    const controls = new OrbitControls(camera, renderer.domElement);
    // 设置控制器阻尼
    controls.enableDamping = true;

    // 渲染函数
    function render() {
      // 更新控制器
      controls.update();
      //   渲染场景
      renderer.render(scene, camera);
      // 循环渲染
      requestAnimationFrame(render);
    }
    render();
  },
};
</script>

<style>
/* * {
  margin: 0;
  padding: 0;
} */
/* .CrystalBear {
  width: 100vw;
  height: 100vh;
} */
</style>
