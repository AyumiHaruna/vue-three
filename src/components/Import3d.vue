<script setup lang="ts">
    import * as THREE from 'three';
    import { GLTFLoader } from 'three/examples/jsm/Addons.js';

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.outputColorSpace = THREE.SRGBColorSpace;

    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.setClearColor(0x000000);
    renderer.setPixelRatio(window.devicePixelRatio);

    document.body.appendChild(renderer.domElement);
    // const wrapper = document.getElementById('scene3d');
    // wrapper.appendChild(renderer.domElement);

    const scene = new THREE.Scene();

    const camera = new THREE.PerspectiveCamera(
        45, window.innerWidth / window.innerHeight, 1, 1000
    );
    camera.position.set(4, 5, 11);
    camera.lookAt(0, 0, 0);

    const groundGeometry = new THREE.PlaneGeometry(20, 20, 32, 32);
    groundGeometry.rotateX(-Math.PI / 2);
    const groundMaterial = new THREE.MeshStandardMaterial({
        color: 0x555555,
        side: THREE.DoubleSide
    });
    const groundMesh = new THREE.Mesh(groundGeometry, groundMaterial);
    scene.add(groundMesh);

    const spotLight = new THREE.SpotLight(0xffffff, 3, 100, 0.2, 0.5);
    spotLight.position.set(0, 25, 0);
    scene.add(spotLight);

    const loader = new GLTFLoader().setPath('/sodaMachine/');
    loader.load('sodaMachine.gltf', (gltf) => {
        const mesh = gltf.scene;
        mesh.position.set(0, 1.01, -1);
        scene.add(mesh);
    });

    const animate = () => {
        requestAnimationFrame(animate);
        renderer.render(scene, camera);
    }

    animate();
</script>

<template>
    <div class="scene3d" id="scene3d">
        test
    </div>
</template>

<style>
    .scene3d {
        width: 100%;
    }
</style>