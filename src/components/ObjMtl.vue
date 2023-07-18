<template>
  <div id="container">1</div>
</template>

<script>
import * as THREE from "three";
// import { TrackballControls } from "three/examples/jsm/controls/TrackballControls";
import { MTLLoader } from "three/examples/jsm/loaders/MTLLoader";
import { OBJLoader } from "three/examples/jsm/loaders/OBJLoader";

export default {
  name: "ObjMtl",
  mounted() {
    var scene = new THREE.Scene();
    var camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    var renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight);
    document.getElementById("container").appendChild(renderer.domElement);

    // 创建obj和mtl加载器
    const objLoader = new OBJLoader();
    const mtlLoader = new MTLLoader();

    mtlLoader.load("/static/JIAN_7(1).mtl", function (materials) {
      materials.preload();
      objLoader.setMaterials(materials);
    });

    objLoader.load("/static/JIAN_7.13.obj", function (object) {
      scene.add(object);
    });

    camera.position.z = 5;

    function animate() {
      requestAnimationFrame(animate);
      renderer.render(scene, camera);
    }

    animate();
  },
  methods: {},
};
</script>

<style></style>
