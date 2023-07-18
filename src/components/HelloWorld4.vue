<template>
  <div id="webgl"></div>
</template>

<script>
import * as THREE from "three";
// 引入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 导入动画库
import gsap from "gsap";
export default {
  name: "HelloWorld4",

  mounted() {
    //js控制全屏

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
      color: "red", //0xff0000设置材质颜色为红色
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
    // console.log(controls);
    // 设置控制器阻尼，让控制器更真实,该值被启用，你将必须在你的动画循环里调用.update()。
    controls.enableDamping = true;

    // 添加坐标轴辅助器
    // AxesHelper：辅助观察的坐标系
    const axesHelper = new THREE.AxesHelper(5);
    scene.add(axesHelper);

    // 设置时钟
    // const clock = new THREE.Clock();

    // 设置动画
    // var animatel = gsap.to(cube.position, {
    //   x: 5,
    //   duration: 5,
    //   // repeat: 3, //动画将进行4次
    //   // 无限次数循环-1
    //   repeat: -1,
    //   // 往返运动
    //   yoyo: true,
    //   // 延迟三秒
    //   delay: 3,
    //   ease: "power1.inOut",
    //   //动画事件
    //   onComplete: () => {
    //     console.log("动画完成");
    //   },
    //   // 动画开始
    //   onStart: () => {
    //     console.log("动画开始");
    //   },
    // });
    gsap.to(cube.rotation, {
      x: 2 * Math.PI,
      duration: 5,
      ease: "power1.inOut",
    });

    window.addEventListener("dblclick", () => {
      // // console.log(animatel, " animatel;");
      // if (animatel.isActive()) {
      //   // 暂停
      //   animatel.pause();
      // } else {
      //   //恢复
      //   animatel.resume();
      // }
      // 双击进入全屏
      const fullScreenElement = document.fullscreenElement;
      if (!fullScreenElement) {
        // 让画布对象全屏
        renderer.domElement.requestFullscreen();
      } else {
        // 退出全屏使用document对象
        document.exitFullscreen();
      }
    });

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
