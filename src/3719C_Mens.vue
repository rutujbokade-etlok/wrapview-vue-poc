<template>
    <div class="h-screen w-full" style="height: 100vh;">
        <Wrapview ref="wrapView" @onInitalized="environmentMounted"></Wrapview>
        <div id="orbitControls"></div>
    </div>
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
    ObjectController
} from "@etlok-systems/wrapview";
import * as THREE from 'three';
import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader.js';

export default {
    components: { Wrapview },
    data() {
        return {
            name: "F_BC3719_MENS_SOLID",
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
                            // this.materials = materials;
                            this.$refs['wrapView'].instance().renderer().toneMapping = THREE.ACESFilmicToneMapping;
                            this.$refs['wrapView'].instance().renderer().toneMappingExposure = 0.768;

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
                var bgColor = 0xe0e0e0;

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

                this.orbitController = new ObjectController(
                    // this.$refs["wrapView"].instance().camera(),
                    document.getElementById("orbitControls"),
                    {
                        allow: {
                            x: false,
                            y: true
                        },
                    }
                );
                this.orbitController.enabled = true;
                // orbitController.enableZoom = true;
                // orbitController.enableDamping = false;
                // orbitController.minDistance = 1;
                // orbitController.maxDistance = 2;

                //Set Initial Camera
                this.resetCamera();

                this.$refs['wrapView'].instance().camera().lookAt(0, 0, 0);
                this.$refs["wrapView"].instance().setController(this.orbitController);
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
                const distance = 150, angle = Math.PI / 4

                var rectAreaLight1 = new WrapviewLight({
                    type: "directional",
                    color: 0xffffff,
                    intensity: 1,
                    position: { x: -Math.cos(Math.PI / 4) * distance, y: 1, z: Math.sin(Math.PI / 4) * distance },
                    width: 30,
                    height: 30,
                });

                var rectAreaLight2 = new WrapviewLight({
                    type: "directional",
                    color: 0xffffff,
                    intensity: 1,
                    position: { x: -distance * Math.cos(angle) * Math.cos(angle), y: distance * Math.sin(angle) * 0.3, z: -distance * Math.cos(angle) * Math.sin(angle) },
                    width: 30,
                    height: 30,
                });

                var rectAreaLight3 = new WrapviewLight({
                    type: "directional",
                    color: 0xffffff,
                    intensity: 1.5,
                    position: { x: 0, y: distance / 2, z: -24 },
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
                    .add(
                        rectAreaLight1,
                        rectAreaLight2,
                        rectAreaLight3,
                        // helper1,
                        // helper2,
                        // helper3
                    );

                const pmremGenerator = new THREE.PMREMGenerator(this.$refs["wrapView"].instance().renderer());
                new RGBELoader().load('/hdr/Light_5.hdr', (hdrTexture) => {
                    const envMap = pmremGenerator.fromEquirectangular(hdrTexture).texture;
                    envMap.mapping = THREE.EquirectangularReflectionMapping;

                    this.$refs["wrapView"].instance().scene().environment = envMap;
                    this.$refs["wrapView"].instance().scene().environmentIntensity = 0.2;
                    // this.$refs["wrapView"].instance().scene().background = envMap; // optional

                    hdrTexture.dispose();
                    pmremGenerator.dispose();
                });
                resolve()
            })

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
                    value: "#0160a1",
                    descriptor: "Black",
                });

                const backNeckTape = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_BACK_NECK_TAPE.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_BACK_NECK_TAPE.png`,
                            normal: `/${this.name}/textures/N_${this.name}_BACK_NECK_TAPE.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_BACK_NECK_TAPE.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const leftArmSleeve = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_LEFT_ARM_SLEEVE.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_LEFT_ARM_SLEEVE.png`,
                            normal: `/${this.name}/textures/N_${this.name}_LEFT_ARM_SLEEVE.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_LEFT_ARM_SLEEVE.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const rightArmSleeve = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_RIGHT_ARM_SLEEVE.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_RIGHT_ARM_SLEEVE.png`,
                            normal: `/${this.name}/textures/N_${this.name}_RIGHT_ARM_SLEEVE.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_RIGHT_ARM_SLEEVE.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const frontBody = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`, // Base layer for text editing
                            diffuse: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            normal: `/${this.name}/textures/N_${this.name}_FRONT_BODY.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_FRONT_BODY.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const backBody = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_BACK_BODY.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_BACK_BODY.png`,
                            normal: `/${this.name}/textures/N_${this.name}_BACK_BODY.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_BACK_BODY.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const leftCuff = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_LEFT_CUFF.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_LEFT_CUFF.png`,
                            normal: `/${this.name}/textures/N_${this.name}_LEFT_CUFF.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_LEFT_CUFF.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const rightCuff = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_RIGHT_CUFF.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_RIGHT_CUFF.png`,
                            normal: `/${this.name}/textures/N_${this.name}_RIGHT_CUFF.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_RIGHT_CUFF.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const hem = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_BOTTOM_HEM.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_BOTTOM_HEM.png`,
                            normal: `/${this.name}/textures/N_${this.name}_BOTTOM_HEM.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_BOTTOM_HEM.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const frontPocket = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_FRONT_POCKET.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_FRONT_POCKET.png`,
                            normal: `/${this.name}/textures/N_${this.name}_FRONT_POCKET.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_FRONT_POCKET.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const innerLeftPockerOpening = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_INNER_LEFT_POCKET_OPENING.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_INNER_LEFT_POCKET_OPENING.png`,
                            normal: `/${this.name}/textures/N_${this.name}_INNER_LEFT_POCKET_OPENING.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_INNER_LEFT_POCKET_OPENING.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const innerRightPocketOpening = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_INNER_RIGHT_POCKET_OPENING.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_INNER_RIGHT_POCKET_OPENING.png`,
                            normal: `/${this.name}/textures/N_${this.name}_INNER_RIGHT_POCKET_OPENING.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_INNER_RIGHT_POCKET_OPENING.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const leftHood = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_LEFT_HOOD.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_LEFT_HOOD.png`,
                            normal: `/${this.name}/textures/N_${this.name}_LEFT_HOOD.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_LEFT_HOOD.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
                    }
                );

                const rightHood = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_RIGHT_HOOD.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_RIGHT_HOOD.png`,
                            normal: `/${this.name}/textures/N_${this.name}_RIGHT_HOOD.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_RIGHT_HOOD.png`,
                        },
                        build: {
                            parameters: {
                                base: true,
                                size: 2048,
                                layers: [],
                                color: color,
                            },
                        },
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
                    backNeckTape.init(),
                    leftArmSleeve.init(),
                    rightArmSleeve.init(),
                    frontBody.init(),
                    backBody.init(),
                    shadow.init(),
                    leftCuff.init(),
                    rightCuff.init(),
                    hem.init(),
                    frontPocket.init(),
                    innerLeftPockerOpening.init(),
                    innerRightPocketOpening.init(),
                    leftHood.init(),
                    rightHood.init(),
                    stitches.init(),
                );

                materials.add("BACK_NECK_TAPE", backNeckTape);
                materials.add("LEFT_ARM_SLEEVE", leftArmSleeve);
                materials.add("RIGHT_ARM_SLEEVE", rightArmSleeve);
                materials.add("FRONT_BODY", frontBody);
                materials.add("BACK_BODY", backBody);
                materials.add("EXT_Stitches", stitches);
                materials.add("LEFT_CUFF", leftCuff);
                materials.add("RIGHT_CUFF", rightCuff);
                materials.add("BOTTOM_HEM", hem);
                materials.add("FRONT_POCKET", frontPocket);
                materials.add("INNER_LEFT_POCKET_OPENING", innerLeftPockerOpening);
                materials.add("INNER_RIGHT_POCKET_OPENING", innerRightPocketOpening);
                materials.add("LEFT_HOOD", leftHood);
                materials.add("RIGHT_HOOD", rightHood);
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
                item.load(`/${this.name}/${this.name}_LOD0.glb`).then(() => {
                    this.orbitController.addObject(item)
                    console.log(item)
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