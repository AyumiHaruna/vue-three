<script setup lang="ts">
    import { 
        Scene, PerspectiveCamera, WebGLRenderer, 
        Mesh, MeshBasicMaterial, SphereGeometry, 
        BoxGeometry} from 'three';
    import { onMounted, ref, computed, watch } from 'vue';
    import { OrbitControls } from 'three/examples/jsm/Addons.js';

    /** 
     * 
     * code from @AlvaroDevLabs youtube channel
     * https://www.youtube.com/watch?v=1mFWG8WBif8
     * 
    */

    // dont use "ref" for the renderer, 
    // it lowers the performance a lot
    let renderer: WebGLRenderer;
    let camera: PerspectiveCamera;
    let controls: OrbitControls;

    const experience = ref<HTMLCanvasElement|null>(null);
    const scene = new Scene();

    const width = ref(window.innerWidth);
    const height = ref(window.innerHeight);
    const aspectRatio = computed(() => width.value / height.value);

    function updateRenderer() {
        renderer.setSize(width.value, height.value);
        renderer.setPixelRatio(window.devicePixelRatio);
    };
    function updateCamera() {
        camera.aspect = aspectRatio.value;
        camera.updateProjectionMatrix();
    };

    window.addEventListener("resize", () => {
        width.value = window.innerWidth;
        height.value = window.innerHeight;
    });
    watch(aspectRatio, updateRenderer);
    watch(aspectRatio, updateCamera);

    camera = new PerspectiveCamera(
        45, 
        aspectRatio.value,
        0.1, 
        1000,
    );    
    camera.position.z = 5;
    scene.add(camera);

    const cube = new Mesh(
    // const sphere = new Mesh(
        // new SphereGeometry(1, 20, 20),
        new BoxGeometry(1, 1, 1, 32, 32),
        new MeshBasicMaterial({ color: 0x008080 }),
    );
    // scene.add(sphere);
    scene.add(cube);

    const loop = () => {
        renderer.render(scene, camera);
        // smooth movement after leave control
        controls.update();
        // sphere.position.y += 0.01;
        requestAnimationFrame(loop);
    };

    onMounted(() => {
        // innert width & height functions are avalible after onMounted
        renderer = new WebGLRenderer({
            canvas: experience.value as unknown as HTMLCanvasElement,
            antialias: true,
        });

        controls = new OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;

        updateRenderer();
        updateCamera();
        
        loop();
    });
    
</script>

<template>
    <canvas ref="experience" />
</template>s