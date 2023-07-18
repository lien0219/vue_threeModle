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
  name: "HelloWorld7",

  mounted() {
    /*三角形
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

    for (let i = 0; i < 50; i++) {
      // 每个三角形需要三个顶点，三个顶点需要三个值
      const geometry = new THREE.BufferGeometry();
      const positionArray = new Float32Array(9);
      for (let j = 0; j < 9; j++) {
        positionArray[j] = Math.random() * 10 - 5;
      }
      geometry.setAttribute(
        "position",
        new THREE.BufferAttribute(positionArray, 3)
      );
      let color = new THREE.Color(Math.random(), Math.random(), Math.random());
      const Material = new THREE.MeshBasicMaterial({
        color: color, //设置材质颜色随机颜色
        transparent: true,
        opacity: 0.5,
      });
      // 根据几何体和材质创建物体
      const mesh = new THREE.Mesh(geometry, Material);
      scene.add(mesh);
    }

    // 创建渲染器对象
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
