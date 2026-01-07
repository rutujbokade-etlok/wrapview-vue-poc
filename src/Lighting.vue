<template>
    <Wrapview class="wrapViewContainer" ref="wrapView" @onInitalized="environmentMounted"></Wrapview>
    <div id="orbitControls"></div>
</template>
<script>
import { Wrapview } from "@etlok-systems/wrapview-vue";
import {
    OrbitControls,
    WrapviewSettings,
    WrapviewObject,
    WrapviewMaterialSet,
    WrapviewShadowMaterial,
    WrapviewTexturedMaterial,
    WrapviewStitchMaterial,
    WrapviewLight,
    WrapviewParameter,
    WrapviewSVGLayer,
    WrapviewInstance,
} from "@etlok-systems/wrapview";
import * as THREE from 'three';

export default {
    components: { Wrapview },
    data() {
        return {
            size: {
                width: window.innerWidth,
                height: window.innerHeight,
            },
        }
    },
    methods: {
        environmentMounted() {
            this.calculateDimensions();
            this.loadEnvironment().then(() =>
                this.loadLights().then(() =>
                    this.loadMaterials().then(({ materials }) =>
                        this.loadObjects(materials).then(() => {
                            this.$refs["wrapView"].show();
                            this.materials = materials;

                            const wrapViewInstance = this.$refs["wrapView"].instance();
                            wrapViewInstance.animate();
                            this.wrapViewInstance = wrapViewInstance;
                            document.getElementsByTagName("canvas")[0].style.height = "100vh";
                        })
                    )
                )
            );
        },
        calculateDimensions() {
            this.size = {
                width: window.innerWidth,
                height: window.innerHeight,
            };
        },
        loadEnvironment() {
            return new Promise((resolve, reject) => {
                WrapviewSettings.init();
                var bgColor = 0xf0f0f0;

                this.$refs["wrapView"].viewer().init({
                    renderer: {
                        antialias: true,
                        alpha: false,
                        preserveDrawingBuffer: true,
                        background: bgColor,
                    },
                    mode: this.mode,
                    agent: this.size,
                });

                var orbitController = new OrbitControls(
                    this.$refs["wrapView"].instance().camera(),
                    document.getElementById("orbitControls")
                );
                orbitController.enabled = true;
                orbitController.enableZoom = true;
                orbitController.enableDamping = false;
                orbitController.minDistance = 1;
                orbitController.maxDistance = 2;

                //Set Initial Camera
                this.resetCamera();

                this.$refs["wrapView"].instance().setController(orbitController);
                resolve();
            });
        },
        resetCamera() {
            let x = 0;
            let y = 0;
            let z = -1.5;
            this.$refs["wrapView"].instance().camera().position.set(x, y, z);
        },
        loadLights() {
            return new Promise((resolve, reject) => {
                const envPaths = [
                    "/environment/light5/px.hdr",
                    "/environment/light5/nx.hdr",
                    "/environment/light5/py.hdr",
                    "/environment/light5/ny.hdr",
                    "/environment/light5/pz.hdr",
                    "/environment/light5/nz.hdr",
                ];

                const envLight = new WrapviewLight({ type: "ambient", intensity:1 });
                this.$refs["wrapView"].instance().scene().add(envLight.createLight());

                envLight
                    .loadHDREnvironmentMap(envPaths)
                    .then((texture) => {
                        this.$refs["wrapView"].instance().scene().environment = texture;
                    })
                    .catch((err) =>
                        console.error("Failed to load environment map:", err)
                    );

                const distance = 150, angle = Math.PI / 4

                var rectAreaLight1 = new WrapviewLight({
                    type: "directional",
                    color: 0xffffff,
                    intensity: 2,
                    position: { x: -Math.cos(Math.PI/4) * distance, y: 1, z: Math.sin(Math.PI/4) * distance },
                    width: 30,
                    height: 30,
                });

                var rectAreaLight2 = new WrapviewLight({
                    type: "directional",
                    color: 0xffffff,
                    intensity: 2,
                    position: { x: -distance * Math.cos(angle) * Math.cos(angle), y: distance * Math.sin(angle)*0.3, z: -distance * Math.cos(angle) * Math.sin(angle) },
                    width: 30,
                    height: 30,
                });

                var rectAreaLight3 = new WrapviewLight({
                    type: "directional",
                    color: 0xffffff,
                    intensity: 1,
                    position: { x: 0, y: distance, z: -10 },
                    width: 30,
                    height: 30,
                });

                rectAreaLight1 = rectAreaLight1.createLight();
                rectAreaLight1.lookAt(0, 0, -1.5);
                rectAreaLight2 = rectAreaLight2.createLight();
                rectAreaLight2.lookAt(0, 0, -1.5);
                rectAreaLight3 = rectAreaLight3.createLight();
                rectAreaLight3.lookAt(0, 0, -1.5);

                const helper1 = new THREE.DirectionalLightHelper(rectAreaLight1, 1, "red");
                const helper2 = new THREE.DirectionalLightHelper(rectAreaLight2, 1, "green");
                const helper3 = new THREE.DirectionalLightHelper(rectAreaLight3, 1, "red");

                this.$refs["wrapView"]
                    .instance()
                    .scene()
                    .add(rectAreaLight1, rectAreaLight2, rectAreaLight3, 
                    helper1, helper2, helper3
                );

                resolve();
            });
        },
        loadMaterials() {
            return new Promise((resolve, reject) => {
                this.$refs["wrapView"].instance().updateOffsets();
                const promises = [];

                var materials = new WrapviewMaterialSet();
                const shadow = new WrapviewShadowMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            alpha:
                                "https://combibmark.s3.amazonaws.com/models/shadow_ultra_light_inverted.png",
                        },
                    }
                );

                var color = new WrapviewParameter(null, "textColor");
                color.set({
                    type: "fixed",
                    value: "#ffffff",
                    descriptor: "Black",
                });

                const collar = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1005.png",
                            diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1005.png",
                            normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1005.png",
                            alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1005.png",
                            // metalness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1005.png",
                            // roughness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1005.png",
                        },
                        // build: {
                        //     parameters: {
                        //         base: true,
                        //         size: 2048,
                        //         layers: [],
                        //         color: color,
                        //     },
                        // },
                    }
                );

                const backNeckTape = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1006.png",
                            diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1006.png",
                            normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1006.png",
                            alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1006.png",
                            // metalness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1006.png",
                            // roughness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1006.png",
                        },
                        // build: {
                        //     parameters: {
                        //         base: true,
                        //         size: 2048,
                        //         layers: [],
                        //         color: color,
                        //     },
                        // },
                    }
                );

                const leftArmSleeve = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1003.png",
                            diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1003.png",
                            normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1003.png",
                            alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1003.png",
                            // metalness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1003.png",
                            // roughness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1003.png",
                        },
                        // build: {
                        //     parameters: {
                        //         base: true,
                        //         size: 2048,
                        //         layers: [],
                        //         color: color,
                        //     },
                        // },
                    }
                );

                const rightArmSleeve = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1004.png",
                            diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1004.png",
                            normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1004.png",
                            alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1004.png",
                            // metalness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1004.png",
                            // roughness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1004.png",
                        },
                        // build: {
                        //     parameters: {
                        //         base: true,
                        //         size: 2048,
                        //         layers: [],
                        //         color: color,
                        //     },
                        // },
                    }
                );

                const frontBody = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: "/3001C_SMALL/textures/F_3001C_SMALL_common.png", // Base layer for text editing
                            diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_common.png",
                            normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1001.png",
                            alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1001.png",
                            // metalness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1001.png",
                            // roughness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1001.png",
                        },
                        // build: {
                        //     parameters: {
                        //         base: true,
                        //         size: 2048,
                        //         layers: [],
                        //         color: color,
                        //     },
                        // },
                    }
                );

                const backBody = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: "/3001C_SMALL/textures/F_3001C_SMALL_common.png",
                            diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_common.png",
                            normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1002.png",
                            alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1002.png",
                            // metalness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1002.png",
                            // roughness:
                            //     "/3001C_SMALL/textures/F_3001C_SMALL_roughness_1002.png",
                        },
                        // build: {
                        //     parameters: {
                        //         base: true,
                        //         size: 2048,
                        //         layers: [],
                        //         color: color,
                        //     },
                        // },
                    }
                );

                const stitches = new WrapviewStitchMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            diffuse: "/3001C_SMALL/textures/Basic_Offset_2193.png",
                        },
                    }
                );

                promises.push(
                    collar.init(),
                    backNeckTape.init(),
                    leftArmSleeve.init(),
                    rightArmSleeve.init(),
                    frontBody.init(),
                    backBody.init(),
                    shadow.init(),
                    stitches.init(),
                );

                materials.add("COLLAR", collar);
                materials.add("BACK_NECK_TAPE", backNeckTape);
                materials.add("LEFT_ARM_SLEEVE", leftArmSleeve);
                materials.add("RIGHT_ARM_SLEEVE", rightArmSleeve);
                materials.add("FRONT_BODY", frontBody);
                materials.add("BACK_BODY", backBody);
                materials.add("EXT_Stitches", stitches);
                materials.add("99_ShadowPanel", shadow);

                const allPromises = Promise.all(promises);
                allPromises.then(
                    () => {
                        resolve({
                            materials: materials,
                        });
                    },
                    (e) => {
                        console.log("Error!", e);
                    }
                );
            });
        },
        loadObjects(materials) {
            return new Promise((resolve, reject) => {
                const item = new WrapviewObject({
                    transform: {
                        rotation: {
                            y: -Math.PI,
                        },
                        position: {
                            y: 0,
                        },
                        scale: {
                            x: 0.8,
                            y: 0.8,
                            z: 0.8,
                        },
                    },
                });
                item.setMaterials(materials);
                item.load("/F_BC3001_MENS_SOLID/model.glb").then(() => {
                    this.$refs["wrapView"].instance().addObject(item);
                });

                resolve();
            });
        }
    }
}

</script>
<style lang="css" scoped>
#orbitControls {
    position: absolute;
    z-index: 999;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100%;
}
</style>