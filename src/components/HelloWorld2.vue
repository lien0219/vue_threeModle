<template>
  <div id="webgl"></div>
</template>

<script>
import * as THREE from "three";
// 引入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
export default {
  name: "HelloWorld2",

  mounted() {
    //clock用于跟踪时间

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
    // 添加物体/几何体
    const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
    const cubeMaterial = new THREE.MeshBasicMaterial({
      color: "yellow", //0xff0000设置材质颜色为红色
    });
    // 根据几何体和材质创建物体
    const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

    // 修改物体位置
    // cube.position.set(5, 0, 0);
    // cube.position.x = 3;

    // 缩放scale
    cube.scale.set(2, 2, 2); // 几何体xyz三个方向都放大2倍
    //cube.scale.x = 3; //可以对x,y,z轴单独设置

    // 旋转.rotation.X()、.rotation.Y()、.rotation.Z()
    // cube.rotation.X(Math.PI / 4);
    // cube.rotation.set(Math.PI / 4, 0, 0);

    //将几何体添加到场景中
    scene.add(cube);
    // 初始化渲染器
    // 创建渲染器对象
    const renderer = new THREE.WebGLRenderer();
    // 定义threejs输出画布的尺寸(单位:像素px)
    // const width = 800; //宽度
    // const height = 500; //高度
    // renderer.setSize(width, height); //设置three.js渲染区域的尺寸(像素px)
    renderer.setSize(window.innerWidth, window.innerHeight);
    //将webgl渲染到canvas内容添加到容器中
    // document.body.appendChild(renderer.document);
    document.getElementById("webgl").appendChild(renderer.domElement);

    // 使用渲染器，通过相机将场景渲染出来
    // renderer.render(scene, camera);

    // 创建轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement);
    console.log(controls);

    // 添加坐标轴辅助器
    // AxesHelper：辅助观察的坐标系
    const axesHelper = new THREE.AxesHelper(5);
    scene.add(axesHelper);

    // 设置时钟
    // const clock = new THREE.Clock();

    function render() {
      // let t = (time / 1000) % 5;
      // // 移动
      // cube.position.x = t * 1;
      // 旋转
      // cube.rotation.x += 0.05;

      // if (cube.position.x > 5) {
      //   cube.position.x = 0;
      // }

      // 获取时钟运行的总时长
      // let time = clock.getElapsedTime();
      // let deltaTime = clock.getDelta();
      // console.log(time, "1");
      // console.log(deltaTime, "2");

      renderer.render(scene, camera);
      // 渲染动画帧
      requestAnimationFrame(render);
    }
    render();
  },
  methods: {},
};
</script>

<style scoped></style>
