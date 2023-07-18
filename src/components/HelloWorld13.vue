<template>
  <div id="webgl"></div>
</template>

<script>
import * as THREE from "three";
// 引入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 导入动画库
// import gsap from "gsap";
//导入dat.gui
// import * as dat from "dat.gui";
export default {
  name: "HelloWorld13",

  mounted() {
    /*设置粗糙度和粗糙度贴图、金属度和金属度贴图、法线贴图
     */

    // 创建场景
    const scene = new THREE.Scene();
    // 创建相机
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    // 设置相机位置
    camera.position.set(0, 0, 10);
    scene.add(camera);

    // 导入纹理
    const textureLoader = new THREE.TextureLoader();
    const doorColorTexture = textureLoader.load("./textures/door/wen1.jpg");
    const doorAplhaTexture = textureLoader.load("./texture/door/wen4.jpg");
    const doorAoTexture = textureLoader.load("./texture/door/wen6.png");
    // 导入置换贴图
    const doorHeightTexture = textureLoader.load("./texture/door/wen5.png");
    // 导入粗糙度贴图
    const roughnessTexture = textureLoader.load("./texture/door/wen3.jpg");
    // 导入金属贴图
    const metalnessTexture = textureLoader.load("./texture/door/wen2.jpg");
    // 导入法线贴图
    const normalTexture = textureLoader.load("./texture/door/wen6.png");

    // 添加物体
    const cubeGeometry = new THREE.BoxGeometry(5, 5, 5);
    // 材质
    const material = new THREE.MeshStandardMaterial({
      color: "white",
      map: doorColorTexture,
      alphaMap: doorAplhaTexture,
      transparent: true,
      // opacity: 0.5,
      aoMap: doorAoTexture,
      aoMapIntensity: 1, //ao强度
      displacementMap: doorHeightTexture, //置换贴图
      displacementScale: 0.1,
      roughness: 1, //粗糙度
      roughnessMap: roughnessTexture, //粗糙度贴图
      metalness: 1, //金属度
      metalnessMap: metalnessTexture, //金属度贴图
      normalMap: normalTexture, //法线贴图
    });
    material.side = THREE.DoubleSide;
    const cube = new THREE.Mesh(cubeGeometry, material);
    scene.add(cube);

    // 添加平面
    const planeGeometry = new THREE.PlaneGeometry(1, 1, 200, 200);
    const plane = new THREE.Mesh(planeGeometry, material);
    plane.position.set(6, 0, 0);
    scene.add(plane);
    // 给平面设置第二组uv
    planeGeometry.setAttribute(
      "uv2",
      new THREE.BufferAttribute(planeGeometry.attributes.uv.array, 2)
    );

    // 灯光
    // 环境光
    const light = new THREE.AmbientLight(0xffffff, 0.5); // soft white light
    scene.add(light);
    // 直线光源
    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    // 设置光源的方向：通过光源position属性和目标指向对象的position属性计算
    directionalLight.position.set(80, 100, 50);
    // 方向光指向对象网格模型mesh，可以不设置，默认的位置是0,0,0
    // directionalLight.target = mesh;
    scene.add(directionalLight);

    // 初始化渲染器
    const renderer = new THREE.WebGLRenderer();

    renderer.setSize(window.innerWidth, window.innerHeight);
    //将webgl渲染到canvas内容添加到容器中
    // document.body.appendChild(renderer.document);
    document.getElementById("webgl").appendChild(renderer.domElement);

    // 创建轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement);
    // console.log(controls);
    // 设置控制器阻尼，让控制器更真实,该值被启用，你将必须在你的动画循环里调用.update()。
    controls.enableDamping = true;

    // 添加坐标轴辅助器
    // AxesHelper：辅助观察的坐标系
    const axesHelper = new THREE.AxesHelper(5);
    scene.add(axesHelper);

    function render() {
      controls.update();
      renderer.render(scene, camera);
      // 渲染动画帧
      requestAnimationFrame(render);
    }
    render();

    // 监听画面变化，更新渲染画面
    window.addEventListener("resize", () => {
      // 更新摄像头
      camera.aspect = window.innerWidth / window.innerHeight;
      // 更新摄像头的投影矩阵
      camera.updateProjectionMatrix();
      // 更新渲染器
      renderer.setSize(window.innerWidth, window.innerHeight);
      // 更新渲染器的像素比
      renderer.setPixelRatio(window.devicePixelRatio);
    });
  },
  methods: {},
};
</script>

<style scoped></style>
