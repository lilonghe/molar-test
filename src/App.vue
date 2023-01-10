<script setup lang="ts">
import * as THREE from 'three';
import { PCDLoader } from 'three/examples/jsm/loaders/PCDLoader'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'

const pcdLoader = new PCDLoader()

const scene = new THREE.Scene();
const pcdResourceUrl = '/model.pcd'
pcdLoader.loadAsync(pcdResourceUrl).then(pcd => {
  pcd.geometry.center();
  pcd.geometry.rotateX( Math.PI );

  scene.add(pcd)
  rerender()
})

const size = {
  width: window.innerWidth,
  height: window.innerHeight
}

const camera = new THREE.PerspectiveCamera( 30, window.innerWidth / window.innerHeight, 0.01, 40 );
camera.position.set( 0, 0, 1 );

const render = new THREE.WebGLRenderer( { antialias: true } );
render.setSize(size.width, size.height);
render.setPixelRatio(window.devicePixelRatio);
render.shadowMap.enabled = true;
render.render(scene, camera);

const rerender = () => {
  render.render(scene, camera);
}

const controls = new OrbitControls( camera, render.domElement );
controls.addEventListener( 'change', () => {
  render.render(scene, camera);
});
controls.minDistance = 0.5;
controls.maxDistance = 10;

document.body.appendChild(render.domElement)
</script>

<template>

</template>

<style scoped>
</style>
