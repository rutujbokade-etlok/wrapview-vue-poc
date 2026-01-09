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
import { HDRLoader } from 'three/examples/jsm/Addons.js';

export default {
    components: { Wrapview },
    data() {
        return {
            name: "F_BC3001_MENS_SOLID",
            size: {
                width: window.innerWidth,
                height: window.innerHeight,
            },
            orbitController: null
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
                            this.$refs['wrapView'].instance().renderer().toneMapping = THREE.NeutralToneMapping;
                            this.$refs['wrapView'].instance().renderer().toneMappingExposure = 2.2;
                            this.$refs['wrapView'].instance().renderer().outputColorSpace = THREE.SRGBColorSpace;

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
                var bgColor = 0xfefefe;

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

                this.orbitController = new OrbitControls(
                    this.$refs["wrapView"].instance().camera(),
                    document.getElementById("orbitControls"),
                    // {
                    //     allow: {
                    //         x: false,
                    //         y: true
                    //     },
                    // }
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

                let physicalShader = THREE.ShaderLib.physical.fragmentShader;

                physicalShader = physicalShader.replace(
                    '\tvec3 outgoingLight = totalDiffuse + totalSpecular + totalEmissiveRadiance;',
                    '\tvec3 outgoingLight = totalDiffuse + totalSpecular + totalEmissiveRadiance;\n' +
                    '\t#ifdef USE_LIGHTMAP\n\n' +
                    '\t\tfloat lightfactor = 0.015;\n' +
                    '\t\tfloat darkfactor = 0.025;\n' +
                    '\t\tif (diffuseColor.r < 0.5) outgoingLight.r += lightMapIrradiance.r * darkfactor;\n' +
                    '\t\telse outgoingLight.r += (1.0 - lightMapIrradiance.r) * lightfactor;\n' +
                    '\t\tif (diffuseColor.g < 0.5) outgoingLight.g += lightMapIrradiance.g * darkfactor;\n' +
                    '\t\telse outgoingLight.g += (1.0 - lightMapIrradiance.g) * lightfactor;\n' +
                    '\t\tif (diffuseColor.b < 0.5) outgoingLight.b += lightMapIrradiance.b * darkfactor;\n' +
                    '\t\telse outgoingLight.b += (1.0 - lightMapIrradiance.b) * lightfactor;\n\n' +
                    '\t#endif\n'
                );

                THREE.ShaderLib.physical.fragmentShader = physicalShader;

                resolve();
            });
        },
        resetCamera() {
            let x = 0;
            let y = 0.0;
            let z = -1.5;
            this.$refs["wrapView"].instance().camera().position.set(x, y, z);
        },
        loadLights() {
            return new Promise((resolve, reject) => {

                function createDirLight(position, intensity, rotation = null) {
                    const dirLight = new THREE.DirectionalLight(new THREE.Color(1, 1, 1), intensity);
                    dirLight.castShadow = false;
                    dirLight.shadow.mapSize = new THREE.Vector2(512, 512);
                    dirLight.shadow.bias = -0.0001;
                    dirLight.shadow.camera.near = 0.03;
                    dirLight.shadow.camera.far = 100;
                    dirLight.shadow.camera.top = 1;
                    dirLight.shadow.camera.bottom = -1;
                    dirLight.shadow.camera.left = -1;
                    dirLight.shadow.camera.right = 1;
                    dirLight.position.copy(position);
                    if (rotation) dirLight.rotation.copy(rotation);
                    else dirLight.lookAt(new THREE.Vector3(0, 0, 0));
                    return dirLight;
                }
                const lightFactor = 0.1;

                const rectLight_01 = createDirLight(new THREE.Vector3(-1.0, 0.5, -2.0), 5. * lightFactor);
                const rectLight_02 = createDirLight(new THREE.Vector3(1.25, 1.25, 2.0), 5. * lightFactor);
                const rectLight_03 = createDirLight(new THREE.Vector3(0, 1.25, 0), 7. * lightFactor, new THREE.Euler(-Math.PI / 2.0, 0, 0));

                this.$refs["wrapView"].instance().scene().add(rectLight_01, rectLight_02, rectLight_03);

                const hdrLoader = new HDRLoader();
                hdrLoader.load("/hdr/Light_5.hdr", (t) => {
                    t.mapping = THREE.EquirectangularReflectionMapping;
                    this.$refs["wrapView"].instance().scene().environment = t;
                    this.$refs["wrapView"].instance().scene().environmentIntensity = 0.15;
                    this.$refs["wrapView"].instance().scene().environmentRotation = -0.174533;
                }, undefined, (e) => console.log(e));
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

                const colors = [
                    { name: "Black", value: "#000" },
                    { name: "Asphalt", value: "#3e3e3e" },
                    { name: "White", value: "#fff" },
                    { name: "Athletic Heather", value: "#a7a8ac" },
                    { name: "Navy", value: "#1d253b" },
                    { name: "True Royal", value: "#415d99" },
                    { name: "Maroon", value: "#5d283e" },
                    { name: "Red", value: "#b30a2c" },
                    { name: "Kelly", value: "#006f4a" },
                    { name: "Pink", value: "#f7c7d2" },
                    { name: "Team Purple", value: "#29193e" },
                    { name: "Gold", value: "#ff981f" },
                    { name: "Forest", value: "#244137" },
                ]

                const index = 12

                var color = new WrapviewParameter(null, "textColor");
                color.set({
                    type: "fixed",
                    value: colors[index].value,
                    descriptor: colors[index].name,
                });

                const collar = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            normal: `/${this.name}/textures/N_${this.name}_COLLAR.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_COLLAR.png`,
                            lightMap: `/${this.name}/textures/L_${this.name}_COLLAR.png`,
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

                const backNeckTape = new WrapviewTexturedMaterial(
                    this.$refs["wrapView"].instance(),
                    {
                        resources: {
                            base: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            normal: `/${this.name}/textures/N_${this.name}_BACK_NECK_TAPE.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_BACK_NECK_TAPE.png`,
                            lightMap: `/${this.name}/textures/L_${this.name}_BACK_NECK_TAPE.png`,
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
                            base: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            normal: `/${this.name}/textures/N_${this.name}_LEFT_ARM_SLEEVE.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_LEFT_ARM_SLEEVE.png`,
                            lightMap: `/${this.name}/textures/L_${this.name}_LEFT_ARM_SLEEVE.png`,
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
                            base: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            normal: `/${this.name}/textures/N_${this.name}_RIGHT_ARM_SLEEVE.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_RIGHT_ARM_SLEEVE.png`,
                            lightMap: `/${this.name}/textures/L_${this.name}_RIGHT_ARM_SLEEVE.png`,
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
                            lightMap: `/${this.name}/textures/L_${this.name}_FRONT_BODY.png`,
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
                            base: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            diffuse: `/${this.name}/textures/T_${this.name}_FRONT_BODY.png`,
                            normal: `/${this.name}/textures/N_${this.name}_BACK_BODY.png`,
                            alpha: `/${this.name}/textures/A_${this.name}_BACK_BODY.png`,
                            lightMap: `/${this.name}/textures/L_${this.name}_BACK_BODY.png`,
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
                        // scale: {
                        //     x: 0.8,
                        //     y: 0.8,
                        //     z: 0.8,
                        // },
                    },
                });
                item.setMaterials(materials);
                item.load(`/${this.name}/${this.name}_LOD0.glb`).then(() => {
                    this.$refs["wrapView"].instance().addObject(item);
                    // this.orbitController.addObject(item)
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