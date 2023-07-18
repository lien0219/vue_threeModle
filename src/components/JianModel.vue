<template>
  <!-- <div ref="container"></div> -->
  <div id="academy"></div>
</template>

<script>
import * as THREE from "three";
import { OBJLoader } from "three/examples/jsm/loaders/OBJLoader";
import { MTLLoader } from "three/examples/jsm/loaders/MTLLoader";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
export default {
  name: "JianModel",
  data() {
    return {
      scene: "",
      light: "",
      camera: "",
      renderer: "",
    };
  },
  mounted() {
    // const scene = new THREE.Scene();
    // const camera = new THREE.PerspectiveCamera(
    //   75,
    //   window.innerWidth / window.innerHeight,
    //   0.1,
    //   1000
    // );
    // camera.position.set(1.5, 1, 1.5);
    // camera.updateProjectionMatrix();
    // const renderer = new THREE.WebGLRenderer();
    // renderer.setSize(window.innerWidth, window.innerHeight);
    // this.$refs.container.appendChild(renderer.domElement);

    // const controls = new OrbitControls(camera, renderer.domElement);
    // controls.enableDamping = true;
    // // controls.enableDamping = true;
    // // controls.enableZoom = true;
    // // controls.autoRotate = false;
    // // controls.autoRotateSpeed = 3;
    // // controls.enablePan = true;
    // // controls.enableKeys = true;
    // // controls.keyPanSpeed = 7;
    // // controls.keys = {
    // //   LEFT: 37,
    // //   UP: 38,
    // //   RIGHT: 39,
    // //   BOTTOM: 40,
    // // };

    // const ambientLight = new THREE.AmbientLight(0xcccccc, 0.4);
    // scene.add(ambientLight);
    // const pointLight = new THREE.PointLight(0xffffff, 0.8);
    // camera.add(pointLight);

    // // 加载mtl文件
    // // const mtlLoader = new MTLLoader();
    // // mtlLoader.load("/static/modle/JIAN_7.mtl", (material) => {
    // //   material.preload();
    // //   // 加载obj文件
    // //   const objLoader = new OBJLoader();
    // //   objLoader.setMaterials(material)
    // //   objLoader.load("/static/modle/JIAN_7.13.obj", (obj) => {
    // //     scene.add(obj);
    // //   });
    // // });

    // const mtlLoader2 = new MTLLoader();
    // const objLoader2 = new OBJLoader();
    // mtlLoader2.load("/static/modle/JIAN_7.mtl", (material) => {
    //   material.preload();
    //   // material.materials.wire_255255255.transparent = true;
    //   // material.materials.wire_255255255.opacity = 0.3;
    //   // material.materials.wire_000000000.transparent = true;
    //   // material.materials.wire_000000000.opacity = 0.3;
    //   // material.materials.wire_115115115.transparent = true;
    //   // material.materials.wire_115115115.opacity = 0.3;
    //   //mtl文件中的材质设置到obj加载器
    //   objLoader2.setMaterials(material);
    //   objLoader2.load("/static/modle/JIAN_7.13.obj", (loadedMesh) => {
    //     // THIS.mesh = loadedMesh;
    //     //设置模型大小
    //     loadedMesh.scale.set(0.08, 0.08, 0.08);
    //     //设置模型位置
    //     loadedMesh.position.set(5, 10, 0);
    //     scene.add(loadedMesh);
    //   });
    //   console.log(objLoader2);
    // });
    // function render() {
    //   requestAnimationFrame(render);
    //   renderer.render(scene, camera);
    // }
    // render();
    this.init();
    this.loadPlant();
    // this.loadPlant2();
    // this.loadPlant3();
    this.animate();
    document.getElementsByTagName("canvas")[0].style.verticalAlign = "bottom"; //解决canvas底部留白问题
    // window.addEventListener("click", this.clickModel, false);
    window.addEventListener("resize", () => {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.setPixelRatio(window.devicePixelRatio);
    });
  },
  methods: {
    /**初始化 */
    initScene() {
      this.scene = new THREE.Scene();
      const ambientLight = new THREE.AmbientLight(0xcccccc, 0.4);
      this.scene.add(ambientLight);
      // 模拟3个坐标轴的对象
      // var axesHelper = new THREE.AxesHelper(15);
      // this.scene.add(axesHelper);
    },
    initCamera() {
      const aspect = window.innerWidth / window.innerHeight; //窗口高度为innerHeight
      this.camera = new THREE.PerspectiveCamera(45, aspect, 1, 1000);
      this.camera.position.set(1.5, 1, 6.5);
      // this.camera.lookAt(new THREE.Vector3(0, 0, 0)); // 让相机指向原点

      const pointLight = new THREE.PointLight(0xffffff, 1, 100);
      pointLight.position.set(0, 0, 1);
      this.scene.add(pointLight);
      this.scene.add(this.camera);
    },
    initRenderer() {
      this.renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.container = document.getElementById("academy");
      this.container.appendChild(this.renderer.domElement);
    },
    // 初始化控制器
    initOrbitControls() {
      let controls = new OrbitControls(this.camera, this.renderer.domElement);
      controls.enableDamping = true;
      // controls.enableZoom = true;
      // controls.autoRotate = false;
      // controls.autoRotateSpeed = 3;
      // controls.enablePan = true;
      // controls.enableKeys = true;
      // controls.keyPanSpeed = 7;
      // controls.keys = {
      //   LEFT: 37,
      //   UP: 38,
      //   RIGHT: 39,
      //   BOTTOM: 40,
      // };
      // this.controls = controls;
    },
    animate() {
      this.renderer.render(this.scene, this.camera);
      this.requestId = requestAnimationFrame(this.animate);
    },
    init() {
      this.group = new THREE.Group();
      this.initScene();
      this.initCamera();
      this.initRenderer();
      this.initOrbitControls();
    },
    /**加载模型 */
    loadPlant() {
      let that = this;
      let objLoader = new OBJLoader();
      let mtlLoader = new MTLLoader();
      //
      mtlLoader.load("/static/JIAN_7(1).mtl", function (materials) {
        // materials.preload();

        objLoader.setMaterials(materials);
        objLoader.load(
          "/static/JIAN_7.13.obj",
          function (obj) {
            //     var material=new THREE.MeshBasicMaterial({
            //     map:new THREE.TextureLoader().load('/static/imgs/logo.png'),
            //     side:THREE.DoubleSide
            // });

            // const material =new THREE.MeshBasicMaterial({
            //   map:new THREE.TextureLoader().load("/static/imgs/logo.png"),
            //   side:THREE.DoubleSide
            // })
            // obj.children.[0].forEach((child,index)=>{
            //   if (index%2==0) {
            //     child.material=material
            //   }else{
            //     child.material=material
            //   }
            //   child.geometry.computeFaceNormals()
            //   child.geometry.computeVerNormals()
            // })

            console.log(obj.children, "obj");
            obj.position.set(0, 0, 0);
            obj.scale.set(0.1, 0.1, 0.1);
            that.scene.add(obj);
          }
          //   called while loading is progressing
          // function (xhr) {
          //   console.log((xhr.loaded / xhr.total) * 100 + "% loaded1111");
          // },
          //   called when loading has errors
          // function (error) {
          //   console.log("An error happened",error);
          // }
        );
      });
    },

    // loadPlant2() {
    //   let that = this;
    //   let objLoader = new OBJLoader();
    //   let mtlLoader = new MTLLoader();
    //   mtlLoader.load("/static/modle/JIAN_7.mtl", function (materials) {
    //     // materials.preload();
    //     objLoader.setMaterials(materials);
    //     objLoader.load(
    //       "/static/modle/JIAN_7.13.obj",
    //       function (obj) {
    //         obj.position.set(0, 0, 0);
    //         obj.scale.set(0.01, 0.01, 0.01);
    //         that.scene.add(obj);
    //       },
    //       //   called while loading is progressing
    //       function (xhr) {
    //         console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
    //       },
    //       //   called when loading has errors
    //       function (error) {
    //         console.log("An error happened1111",error);
    //       }
    //     );
    //   });
    // },

    // loadPlant3() {
    //   let that = this;
    //   let objLoader = new OBJLoader();
    //   let mtlLoader = new MTLLoader();
    //   mtlLoader.load("/static/academic/绿树1.mtl", function (materials) {
    //     // materials.preload();
    //     objLoader.setMaterials(materials);
    //     objLoader.load(
    //       "/static/academic/绿树1.obj",
    //       function (obj) {
    //         obj.position.set(10, 10, 0);
    //         obj.scale.set(0.01, 0.01, 0.01);
    //         that.scene.add(obj);
    //       },
    //       //   called while loading is progressing
    //       function (xhr) {
    //         console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
    //       },
    //       //   called when loading has errors
    //       function (error) {
    //         console.log("An error happened",error);
    //       }
    //     );
    //   });
    // },
  },
};
</script>

<style></style>
