<template>
  <div class="home">
    <h1>Hello there</h1>
    <div id="canvas"></div>
  </div>
</template>

<style scoped>
#canvas {
  width: 60vw;
  height: 50vh;
  text-align: center;
  margin: auto auto;
}
</style>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

export default {
  name: "Home",
  mounted: function () {
    this.threeJS();
  },
  methods: {
    threeJS: function () {
      // scene and camera
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      camera.position.z = 4;

      // renderer
      const renderer = new THREE.WebGLRenderer({
        // alpha: true,
      });
      const container = document.getElementById("canvas");
      const width = container.getBoundingClientRect().width;
      const height = container.getBoundingClientRect().height;
      renderer.setSize(width, height);
      container.appendChild(renderer.domElement);

      // lighting
      const ambientLight = new THREE.AmbientLight();
      ambientLight.intensity = 0.2;
      scene.add(ambientLight);

      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
      directionalLight.position.set(1, 0, 1);
      scene.add(directionalLight);

      // earth
      const textureLoader = new THREE.TextureLoader();
      const texture = textureLoader.load("/earth_day.jpeg");
      const normalMap = textureLoader.load("/earth_normal_map.png");
      const geometry = new THREE.SphereGeometry(2, 2048, 1024);
      const material = new THREE.MeshStandardMaterial();
      material.map = texture;
      material.normalMap = normalMap;
      const earth = new THREE.Mesh(geometry, material);
      scene.add(earth);

      // camera controls
      const controls = new OrbitControls(camera, renderer.domElement);
      controls.maxPolarAngle = 0.75 * Math.PI;
      controls.minPolarAngle = 0.25 * Math.PI;
      controls.minDistance = 3;
      controls.update();

      // recursive animation using browser framerate
      function animate() {
        requestAnimationFrame(animate);

        earth.rotateY(0.005);
        controls.update();
        renderer.render(scene, camera);
      }
      animate();
    },
  },
};
</script>
