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

export default {
  name: "Home",
  mounted: function () {
    this.threeJS();
  },
  methods: {
    threeJS: function () {
      const scene = new THREE.Scene();
      const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      const renderer = new THREE.WebGLRenderer();
      const container = document.getElementById("canvas");
      const width = container.getBoundingClientRect().width;
      const height = container.getBoundingClientRect().height;
      renderer.setSize(width, height);
      container.appendChild(renderer.domElement);

      const ambientLight = new THREE.AmbientLight(0x404040); // soft white light
      scene.add(ambientLight);

      const directionalLight = new THREE.DirectionalLight(0xffffff, 0.9);
      directionalLight.position.set(10, 20, 0);
      scene.add(directionalLight);

      var texture = new THREE.TextureLoader().load("/earth_day.jpeg");
      console.log(texture);
      const geometry = new THREE.SphereGeometry(2, 64, 32);
      const material = new THREE.MeshBasicMaterial({ map: texture });
      const sphere = new THREE.Mesh(geometry, material);
      scene.add(sphere);

      camera.position.z = 5;

      function animate() {
        requestAnimationFrame(animate);

        // sphere.rotation.x += 0.01;
        sphere.rotation.y += 0.005;

        renderer.render(scene, camera);
      }
      animate();
    },
  },
};
</script>
