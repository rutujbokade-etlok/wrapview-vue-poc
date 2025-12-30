<template>
  <div class="header">
    <div><img src="/icons/Arrow.png" alt="" srcset="" /></div>
    <div>
      <img src="/icons/Logo.png" alt="" srcset="" />
    </div>
    <div></div>
  </div>
  <Wrapview
    class="wrapViewContainer"
    ref="wrapView"
    @onInitalized="environmentMounted"
  ></Wrapview>
  <div id="orbitControls"></div>
  <section class="bottom-panel">
    <div class="svg-preview-panel" style="display: none">
      <div id="editor-panel"></div>
    </div>
    <div class="thumb-container">
      <div class="thumb"></div>
    </div>
    <div class="top-tabs">
      <div class="tab">
        <img src="/icons/Colors.png" />
      </div>
      <div class="tab">
        <img src="/icons/Layers.png" />
      </div>
      <div class="tab active">
        <img src="/icons/TextEditor.png" />
      </div>
      <div class="tab">
        <img src="/icons/Images.png" />
      </div>
      <div class="tab">
        <img src="/icons/Personalize.png" />
      </div>
      <div class="tab">
        <img src="/icons/Done.png" />
      </div>
    </div>
    <div class="bottom-tabs">
      <div
        class="tab"
        :class="{ active: activeTab === 0 }"
        v-on:click="changeTab(0)"
      >
        <img src="/icons/Edit.svg" />
        <p>Edit Text</p>
      </div>
      <div
        class="tab"
        :class="{ active: activeTab === 1 }"
        v-on:click="changeTab(1)"
      >
        <img src="/icons/EditColor.svg" style="height: 24px; width: 24px" />
        <p>Edit Color</p>
      </div>
      <div
        class="tab"
        :class="{ active: activeTab === 2 }"
        v-on:click="changeTab(2)"
      >
        <img src="/icons/Aa.svg" />
        <p>Font</p>
      </div>
      <div
        class="tab"
        :class="{ active: activeTab === 3 }"
        v-on:click="changeTab(3)"
      >
        <img src="/icons/AOutline.svg" />
        <p>Outline</p>
      </div>
      <div
        class="tab"
        :class="{ active: activeTab === 4 }"
        v-on:click="changeTab(4)"
      >
        <img src="/icons/Arch.svg" />
        <p>Shape</p>
      </div>
    </div>
    <div class="content">
      <div id="tab1" :class="{ 'display-none': activeTab !== 0 }">
        <div
          style="
            padding: 10px;
            display: flex;
            flex-direction: column;
            flex-wrap: wrap;
            gap: 15px;
          "
        >
          <label style="flex: 1">
            <span style="display: block; margin-bottom: 5px; font-weight: bold"
              >Edit Text</span
            >
            <input
              type="text"
              v-model="svgTextValue"
              placeholder="Enter text"
              style="
                width: 100%;
                padding: 8px;
                border: 1px solid #ccc;
                border-radius: 4px;
              "
            />
          </label>
          <label style="flex: 1; width: 100%">
            <span style="display: block; margin-bottom: 5px; font-weight: bold"
              >Text Size (px)</span
            >
            <!-- <input type="number" v-model.number="svgFontSize" min="8" max="72" step="1"
							style="width: 100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px;" /> -->
            <input
              type="range"
              v-model="svgFontSize"
              min="50"
              max="250"
              class="styled-range w-full"
            />
          </label>
        </div>
      </div>
      <div id="tab2" :class="{ 'display-none': activeTab !== 1 }">
        <div class="color-container">
          <div
            class="color"
            style="background-color: #ffffff; border: 1px solid #dddddd"
            @click="svgTextColor = '#FFFFFF'"
            title="White"
          ></div>
          <div
            class="color"
            style="background-color: #000000"
            @click="svgTextColor = '#000000'"
            title="Black"
          ></div>
          <div
            class="color"
            style="
              background: linear-gradient(
                  45deg,
                  #ccc 25%,
                  transparent 25%,
                  transparent 75%,
                  #ccc 75%,
                  #ccc
                ),
                linear-gradient(
                  45deg,
                  #ccc 25%,
                  transparent 25%,
                  transparent 75%,
                  #ccc 75%,
                  #ccc
                );
              background-size: 8px 8px;
              background-position: 0 0, 4px 4px;
              border: 1px solid #dddddd;
            "
            @click="svgTextColor = 'transparent'"
            title="Transparent"
          ></div>
          <div
            class="color"
            style="background-color: #595f5e"
            @click="svgTextColor = '#595F5E'"
            title="Dark Gray"
          ></div>
          <div
            class="color"
            style="background-color: #ff0086"
            @click="svgTextColor = '#FF0086'"
            title="Hot Pink"
          ></div>
          <div
            class="color"
            style="background-color: #ffa5b6"
            @click="svgTextColor = '#FFA5B6'"
            title="Light Pink"
          ></div>
          <div
            class="color"
            style="background-color: #ff80b6"
            @click="svgTextColor = '#FF80B6'"
            title="Medium Pink"
          ></div>
          <div
            class="color"
            style="background-color: #ad3d80"
            @click="svgTextColor = '#AD3D80'"
            title="Purple Pink"
          ></div>
          <div
            class="color"
            style="background-color: #732735"
            @click="svgTextColor = '#732735'"
            title="Maroon"
          ></div>
          <div
            class="color"
            style="background-color: #c30c27"
            @click="svgTextColor = '#C30C27'"
            title="Red"
          ></div>
          <div
            class="color"
            style="background-color: #ef2b3b"
            @click="svgTextColor = '#EF2B3B'"
            title="Bright Red"
          ></div>
          <div
            class="color"
            style="background-color: #ff761c"
            @click="svgTextColor = '#FF761C'"
            title="Orange"
          ></div>
          <div
            class="color"
            style="background-color: #ffab29"
            @click="svgTextColor = '#FFAB29'"
            title="Light Orange"
          ></div>
          <div
            class="color"
            style="background-color: #ffc032"
            @click="svgTextColor = '#FFC032'"
            title="Gold Orange"
          ></div>
          <div
            class="color"
            style="background-color: #ffd725"
            @click="svgTextColor = '#FFD725'"
            title="Yellow"
          ></div>
          <div
            class="color"
            style="background-color: #f9e85c"
            @click="svgTextColor = '#F9E85C'"
            title="Light Yellow"
          ></div>
          <div
            class="color"
            style="background-color: #815e24"
            @click="svgTextColor = '#815E24'"
            title="Brown"
          ></div>
          <div
            class="color"
            style="background-color: #60c8a9"
            @click="svgTextColor = '#60C8A9'"
            title="Mint"
          ></div>
          <div
            class="color"
            style="background-color: #98dd22"
            @click="svgTextColor = '#98DD22'"
            title="Lime Green"
          ></div>
          <div
            class="color"
            style="background-color: #289b24"
            @click="svgTextColor = '#289B24'"
            title="Green"
          ></div>
          <div
            class="color"
            style="background-color: #005e31"
            @click="svgTextColor = '#005E31'"
            title="Dark Green"
          ></div>
          <div
            class="color"
            style="background-color: #27472d"
            @click="svgTextColor = '#27472D'"
            title="Forest Green"
          ></div>
          <div
            class="color"
            style="background-color: #00686c"
            @click="svgTextColor = '#00686C'"
            title="Teal"
          ></div>
          <div
            class="color"
            style="background-color: #0091aa"
            @click="svgTextColor = '#0091AA'"
            title="Cyan"
          ></div>
          <div
            class="color"
            style="background-color: #1da6eb"
            @click="svgTextColor = '#1DA6EB'"
            title="Sky Blue"
          ></div>
          <div
            class="color"
            style="background-color: #86b6e6"
            @click="svgTextColor = '#86B6E6'"
            title="Light Blue"
          ></div>
          <div
            class="color"
            style="background-color: #045594"
            @click="svgTextColor = '#045594'"
            title="Blue"
          ></div>
          <div
            class="color"
            style="background-color: #0e3eb0"
            @click="svgTextColor = '#0E3EB0'"
            title="Royal Blue"
          ></div>
          <div
            class="color"
            style="background-color: #073575"
            @click="svgTextColor = '#073575'"
            title="Dark Blue"
          ></div>
          <div
            class="color"
            style="background-color: #032c4f"
            @click="svgTextColor = '#032C4F'"
            title="Navy"
          ></div>
          <div
            class="color"
            style="background-color: #a271bf"
            @click="svgTextColor = '#A271BF'"
            title="Light Purple"
          ></div>
          <div
            class="color"
            style="background-color: #b523b1"
            @click="svgTextColor = '#B523B1'"
            title="Purple"
          ></div>
        </div>
      </div>
      <div id="tab3" :class="{ 'display-none': activeTab !== 2 }">
        <div
          class="font-variants"
          :class="{ 'active-font': svgFontFamily === 'Impact' }"
          @click="svgFontFamily = 'Impact'"
        >
          <h4
            style="font-weight: 600; font-family: 'Impact', sans-serif"
          >
            {{ svgTextValue }}
          </h4>
          <p>Impact</p>
        </div>
        <div
          class="font-variants"
          :class="{ 'active-font': svgFontFamily === 'Baloo 2' }"
          @click="svgFontFamily = 'Baloo 2'"
        >
          <h4 style="font-weight: 600; font-family: 'Baloo 2', sans-serif">
            {{ svgTextValue }}
          </h4>
          <p>Baloo</p>
        </div>
        <div
          class="font-variants"
          :class="{ 'active-font': svgFontFamily === 'Caprasimo' }"
          @click="svgFontFamily = 'Caprasimo'"
        >
          <h4 style="font-weight: 600; font-family: 'Caprasimo', serif">
            {{ svgTextValue }}
          </h4>
          <p>Caprasimo</p>
        </div>
        <div
          class="font-variants"
          :class="{ 'active-font': svgFontFamily === 'Caramel' }"
          @click="svgFontFamily = 'Caramel'"
        >
          <h4 style="font-weight: 600; font-family: 'Caramel', cursive">
            {{ svgTextValue }}
          </h4>
          <p>Caramel</p>
        </div>
      </div>
      <div id="tab4" :class="{ 'display-none': activeTab !== 3 }">
        <div class="outline-header">
          <div>
            <p>Enable Outline</p>
            <input
              type="checkbox"
              v-model="svgOutlineEnabled"
              style="width: 20px; height: 20px; cursor: pointer"
            />
          </div>
          <p>{{ svgOutlineThickness }}pt</p>
        </div>
        <div class="color-container">
          <div
            class="color"
            style="background-color: #ffffff; border: 1px solid #dddddd"
            @click="svgOutlineColor = '#FFFFFF'"
            title="White"
          ></div>
          <div
            class="color"
            style="background-color: #000000"
            @click="svgOutlineColor = '#000000'"
            title="Black"
          ></div>
          <div
            class="color"
            style="background-color: #595f5e"
            @click="svgOutlineColor = '#595F5E'"
            title="Dark Gray"
          ></div>
          <div
            class="color"
            style="background-color: #c30c27"
            @click="svgOutlineColor = '#C30C27'"
            title="Red"
          ></div>
          <div
            class="color"
            style="background-color: #ff761c"
            @click="svgOutlineColor = '#FF761C'"
            title="Orange"
          ></div>
          <div
            class="color"
            style="background-color: #ffd725"
            @click="svgOutlineColor = '#FFD725'"
            title="Yellow"
          ></div>
          <div
            class="color"
            style="background-color: #289b24"
            @click="svgOutlineColor = '#289B24'"
            title="Green"
          ></div>
          <div
            class="color"
            style="background-color: #0091aa"
            @click="svgOutlineColor = '#0091AA'"
            title="Cyan"
          ></div>
          <div
            class="color"
            style="background-color: #045594"
            @click="svgOutlineColor = '#045594'"
            title="Blue"
          ></div>
          <div
            class="color"
            style="background-color: #b523b1"
            @click="svgOutlineColor = '#B523B1'"
            title="Purple"
          ></div>
          <div
            class="color"
            style="background-color: #ad3d80"
            @click="svgOutlineColor = '#AD3D80'"
            title="Purple Pink"
          ></div>
        </div>
        <div
          class="outline-footer"
          style="
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
          "
        >
          <p style="margin: 0">Outline Thickness (pt)</p>
          <!-- <input type="number" v-model.number="svgOutlineThickness" min="0" max="10" step="0.01"
						style="width: 80px; padding: 8px; border: 1px solid #ccc; border-radius: 4px;" /> -->
          <input
            type="range"
            v-model="svgOutlineThickness"
            min="0"
            max="10"
			step="0.1"
            class="styled-range w-full"
          />
        </div>
      </div>
      <div id="tab5" :class="{ 'display-none': activeTab !== 4 }">
        <div class="style-content">
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'none' }"
            @click="svgTextShape = 'none'"
          >
            <h2>None</h2>
          </div>
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'arch' }"
            @click="svgTextShape = 'arch'"
          >
            <img src="/icons/arch.png" />
          </div>
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'Adistort' }"
            @click="svgTextShape = 'Adistort'"
          >
            <img src="/icons/wave.png" />
          </div>
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'bulge' }"
            @click="svgTextShape = 'bulge'"
          >
            <img src="/icons/buldge.png" />
          </div>
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'flag' }"
            @click="svgTextShape = 'flag'"
          >
            <img src="/icons/flag.png" />
          </div>
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'distort' }"
            @click="svgTextShape = 'distort'"
          >
            <img src="/icons/angle.png" />
          </div>
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'circle' }"
            @click="svgTextShape = 'circle'"
          >
            <img src="/icons/circle.png" />
          </div>
          <div
            class="fontStyle"
            :class="{ 'active-shape': svgTextShape === 'pinch' }"
            @click="svgTextShape = 'pinch'"
          >
            <img src="/icons/pinch.png" />
          </div>
        </div>
        <div class="style-footer">
          <p>Shape Intensity: {{ svgShapeIntensity }}%</p>
          <input
            type="range"
            v-model="svgShapeIntensity"
            min="0"
            max="100"
            class="styled-range"
          />
        </div>
      </div>
    </div>
  </section>
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
  WrapviewVectorSvgTextLayer,
  WrapviewInstance,
  WrapviewUtils,
} from "@etlok-systems/wrapview";

// Constants
const TEXTURE_SIZE = 2048;
const LAYER_SIZE = { width: 480, height: 480 };
const MODEL_PATH = "/3001C_SMALL/3001C_SMALL_LOD0.glb";
const TEXTURE_BASE_PATH = "/3001C_SMALL/textures";

// Material configurations for textured panels
const TEXTURED_MATERIALS_CONFIG = {
  COLLAR: { suffix: 1005, base: "F_3001C_SMALL_diffuse_1005.png" },
  BACK_NECK_TAPE: { suffix: 1006, base: "F_3001C_SMALL_diffuse_1006.png" },
  LEFT_ARM_SLEEVE: { suffix: 1003, base: "F_3001C_SMALL_diffuse_1003.png" },
  RIGHT_ARM_SLEEVE: { suffix: 1004, base: "F_3001C_SMALL_diffuse_1004.png" },
  FRONT_BODY: { suffix: 1001, base: "F_3001C_SMALL_common.png" },
  BACK_BODY: { suffix: 1002, base: "F_3001C_SMALL_common.png" },
};

// Effect button configurations
const EFFECT_BUTTONS_CONFIG = [
  { id: "apply-none-effect-btn", effect: "none" },
  { id: "apply-arch-effect-btn", effect: "arch" },
  { id: "apply-flag-effect-btn", effect: "flag" },
  { id: "apply-bulge-effect-btn", effect: "bulge" },
  { id: "apply-pinch-effect-btn", effect: "pinch" },
];

export default {
  components: { Wrapview },

  data() {
    return {
      materials: null,
      wrapViewInstance: null,
      vectorTextLayer: null,
      svgEditor: null,
      debounceTimer: null,
      debouncedApplyTexture: null,
      currentEffect: "none",
      size: {
        height: 0,
        width: 0,
      },
      activeTab: 0,
      svgShapes: [],
      svgTextColor: "#FFAB29",
      svgFontSize: 100,
      svgTextDecoration: "",
      svgFontFamily: "Big Shoulders Stencil",
      svgTextValue: "WARRIORS",
      svgTextShape: "arch",
      svgShapeIntensity: 50,
      svgOutlineColor: "#ffffff",
      svgOutlineThickness: 0.5,
      svgOutlineEnabled: true,
      svgInitialized: false,
      currentSvgLayer: null,
      svgCharSpacing: 0,
    };
  },
  mounted() {
    this.$nextTick(() => {
      // Initialize debounced texture application
      this.debouncedApplyTexture = this.debounce(
        this.applyTextTextureToPanels,
        300
      );
    });
  },
  watch: {
    svgTextColor(val) {
      if (this.vectorTextLayer && this.vectorTextLayer.setFontColor) {
        this.vectorTextLayer.setFontColor(val);
        this.renderEffectAndApplyTexture();
      }
    },
    svgFontSize(val) {
      if (this.vectorTextLayer && this.vectorTextLayer.setFontSize) {
        this.vectorTextLayer.setFontSize(parseInt(val));
        this.renderEffectAndApplyTexture();
      } else {
        console.warn("⚠️ Vector text layer or setFontSize method not available");
      }
    },
    svgTextDecoration(val) {
      this.renderEffectAndApplyTexture();
    },
    svgFontFamily(val) {
      if (this.vectorTextLayer && this.vectorTextLayer.setFontFamily) {
        this.vectorTextLayer.setFontFamily(val);
        this.renderEffectAndApplyTexture();
      }
    },
    svgTextValue(val) {
      if (this.vectorTextLayer && this.vectorTextLayer.setText) {
        this.vectorTextLayer.setText(val);
        this.renderEffectAndApplyTexture();
      }
    },
    svgTextShape(val) {
      this.currentEffect = val;
      this.renderEffectAndApplyTexture();
    },
    svgShapeIntensity(val) {
      if (this.vectorTextLayer && this.vectorTextLayer.setShapeIntensity) {
        this.vectorTextLayer.setShapeIntensity(val);
        this.renderEffectAndApplyTexture();
      }
    },
    svgOutlineColor(val) {
      this.renderEffectAndApplyTexture();
    },
    svgOutlineThickness(val) {
      if (this.vectorTextLayer && this.vectorTextLayer.setOutlineThickness) {
        this.vectorTextLayer.setOutlineThickness(val);
        this.renderEffectAndApplyTexture();
      }
    },
    svgOutlineEnabled(val) {
      this.updateSvgEditorOutline();
      this.renderEffectAndApplyTexture();
    },
    svgCharSpacing(val) {
      if (this.vectorTextLayer && this.vectorTextLayer.setCharSpacing) {
        this.vectorTextLayer.setCharSpacing(val);
        this.renderEffectAndApplyTexture();
      }
    },
  },
  methods: {
    changeTab(tab) {
      this.activeTab = tab;
    },
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

              // Initialize SVG editor after wrapViewInstance is set
              this.initializeSvgEditor();
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
        var bgColor = 0xf3f4f6;

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
        orbitController.enableZoom = false;
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
        // const dirLight = new WrapviewLight({
        // 	type: "directional",
        // 	color: 0xffffff,
        // 	intensity: 0.5,
        // 	position: { x: 1, y: 1, z: -2 },
        // 	target: { x: 0, y: 0, z: 0 },
        // });

        // const hemLight = new WrapviewLight({
        // 	type: "hemisphere",
        // 	color: 0xffffff,
        // 	intensity: 1,
        // });

        // this.$refs["wrapView"]
        // 	.instance()
        // 	.scene()
        // 	.add(hemLight.createLight(), dirLight.createLight());

        // const envPaths = [
        // 	"/environment/px.jpg",
        // 	"/environment/nx.jpg",
        // 	"/environment/py.jpg",
        // 	"/environment/ny.jpg",
        // 	"/environment/pz.jpg",
        // 	"/environment/nz.jpg",
        // ];

        // const envLight = new WrapviewLight({ type: "ambient", intensity: 1 });
        // this.$refs["wrapView"].instance().scene().add(envLight.createLight());

        // envLight
        // 	.loadEnvironmentMap(envPaths)
        // 	.then((texture) => {
        // 		this.$refs["wrapView"].instance().scene().environment = texture;
        // 	})
        // 	.catch((err) =>
        // 		console.error("Failed to load environment map:", err)
        // );

        var rectAreaLight1 = new WrapviewLight({
          type: "rectarea",
          color: 0xffffff,
          intensity: 3,
          position: { x: 4, y: 25, z: 9 },
          width: 30,
          height: 30,
        });

        var rectAreaLight2 = new WrapviewLight({
          type: "rectarea",
          color: 0xffffff,
          intensity: 3,
          position: { x: 4, y: 12, z: -8 },
          width: 30,
          height: 30,
        });

        var rectAreaLight3 = new WrapviewLight({
          type: "rectarea",
          color: 0xffffff,
          intensity: 4,
          position: { x: 0, y: 55, z: 0 },
          width: 30,
          height: 30,
        });

        rectAreaLight1 = rectAreaLight1.createLight();
        rectAreaLight1.lookAt(0, 0, 0);
        rectAreaLight2 = rectAreaLight2.createLight();
        rectAreaLight2.lookAt(0, 0, 0);
        rectAreaLight3 = rectAreaLight3.createLight();
        rectAreaLight3.lookAt(0, 0, 0);

        this.$refs["wrapView"]
          .instance()
          .scene()
          .add(rectAreaLight1, rectAreaLight2, rectAreaLight3);

        resolve();
      });
    },
    loadMaterials() {
      return new Promise((resolve, reject) => {
        this.$refs["wrapView"].instance().updateOffsets();

        // Initialize text color parameter
        var color = new WrapviewParameter(null, "textColor");
        color.set({
          type: "fixed",
          value: "#2b2b2b",
          descriptor: "Black",
        });

        // Factory function to create textured materials
        const createTexturedMaterial = (config) => {
          const { suffix, base } = config;
          return new WrapviewTexturedMaterial(
            this.$refs["wrapView"].instance(),
            {
              resources: {
                base: `${TEXTURE_BASE_PATH}/${base}`,
                diffuse: `${TEXTURE_BASE_PATH}/${base}`,
                normal: `${TEXTURE_BASE_PATH}/F_3001C_SMALL_normal_${suffix}.png`,
                alpha: `${TEXTURE_BASE_PATH}/F_3001C_SMALL_opacity_${suffix}.png`,
                metalness: `${TEXTURE_BASE_PATH}/F_3001C_SMALL_metalness_${suffix}.png`,
              },
              build: {
                parameters: {
                  base: true,
                  size: TEXTURE_SIZE,
                  layers: [],
                  color: color,
                },
              },
            }
          );
        };

        // Create all textured materials
        const texturedMaterials = {};
        Object.entries(TEXTURED_MATERIALS_CONFIG).forEach(([name, config]) => {
          texturedMaterials[name] = createTexturedMaterial(config);
        });

        // Create special materials
        const shadow = new WrapviewShadowMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              alpha: "https://combibmark.s3.amazonaws.com/models/shadow_ultra_light_inverted.png",
            },
          }
        );

        const stitches = new WrapviewStitchMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              diffuse: `${TEXTURE_BASE_PATH}/Basic_Offset_2193.png`,
            },
          }
        );

        // Initialize all materials in parallel
        const promises = [
          ...Object.values(texturedMaterials).map((m) => m.init()),
          shadow.init(),
          stitches.init(),
        ];

        // Add all materials to the set
        const materials = new WrapviewMaterialSet();
        Object.entries(texturedMaterials).forEach(([name, material]) => {
          materials.add(name, material);
        });
        materials.add("EXT_Stitches", stitches);
        materials.add("99_ShadowPanel", shadow);

        Promise.all(promises).then(
          () => {
            resolve({
              materials: materials,
            });
          },
          (e) => {
            console.log("Error!", e);
            reject(e);
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
              y: 0.16,
            },
            scale: {
              x: 0.8,
              y: 0.8,
              z: 0.8,
            },
          },
        });
        item.setMaterials(materials);
        item.load("/3001C_SMALL/3001C_SMALL_LOD0.glb").then(() => {
          this.$refs["wrapView"].instance().addObject(item);
        });

        resolve();
      });
    },
    currentPanel() {
      // Get the material from the materials set using the proper getter method
      // A "panel" in this context is a WrapviewTexturedMaterial that can have text layers
      const panel = this.materials.get("FRONT_BODY");
      if (!panel) {
        console.error("FRONT_BODY panel not found in materials");
        return null;
      }
      return panel;
    },
    debounce(func, delay) {
      return (...args) => {
        clearTimeout(this.debounceTimer);
        this.debounceTimer = setTimeout(() => func(...args), delay);
      };
    },
    async applyTextTextureToPanels(dataUrl) {
      if (!dataUrl) {
        console.warn("No data URL provided for texture");
        return;
      }

      try {
        const panel = this.currentPanel();
        if (!panel) {
          console.error("Cannot apply texture: panel not found");
          return;
        }

        if (!panel.texture()) {
          console.error("Cannot add SVG layer: texture not initialized");
          return;
        }

        const size = panel.settings.build?.parameters?.size || TEXTURE_SIZE;

        if (!this.currentSvgLayer) {
          this.currentSvgLayer = new WrapviewSVGLayer(WrapviewUtils.guid(), {
            size: LAYER_SIZE,
            pivot: { x: 0.5, y: 0.5 },
            position: { x: size / 2, y: size / 2 },
            angle: 0,
          });
        }

        const layer = this.currentSvgLayer;
        const texture = panel.texture();

        await texture.beginEditing();
        try {
          const layers = texture.layers();
          let layerIndex = layers.findIndex((l) => l.id === layer.id);
          if (layerIndex === -1) {
            layerIndex = texture.addLayer(layer);
          }

          await layer.load(
            {
              svgData: dataUrl,
            },
            panel
          );

          texture.editLayer(layerIndex);
          texture.render();
        } catch (error) {
          console.error("Error loading text layer:", error);
        } finally {
          await texture.endEditing();
        }

        console.log("Text texture applied to garment panel");
      } catch (error) {
        console.error("Error applying text texture:", error);
      }
    },
    initializeSvgEditor() {
      if (!this.wrapViewInstance) {
        console.error("❌ wrapViewInstance not initialized");
        return;
      }

      const panel = this.currentPanel();
      if (!panel) {
        console.error("Front body panel not found");
        return;
      }

      const size = panel.settings.build?.parameters?.size || TEXTURE_SIZE;

      // Initialize vector text layer instead of SVG editor
      this.vectorTextLayer = new WrapviewVectorSvgTextLayer(
        WrapviewUtils.guid(),
        {
          text: this.svgTextValue,
          fontFamily: this.svgFontFamily,
          fontVariant: "regular",
          fontSize: this.svgFontSize,
          fontColor: this.svgTextColor,
          size: LAYER_SIZE,
          pivot: { x: 0.5, y: 0.5 },
          position: { x: size / 2, y: size / 2 },
          angle: 0,
          effect: this.svgTextShape,
          outline: {
            include: this.svgOutlineEnabled,
            color: this.svgOutlineColor,
            thickness: this.svgOutlineThickness,
          },
        }
      );

      this.vectorTextLayer.setApiKey("");

      this.vectorTextLayer
        .load(null, panel)
        .then(() => {
          this.currentEffect = this.svgTextShape;
          this.svgInitialized = true;
          console.log("✅ Vector text layer initialized successfully");
          setTimeout(() => {
            this.updateGarmentTexture();
          }, 500);
        })
        .catch((error) => {
          console.error("Failed to initialize vector text layer:", error);
        });
    },
    updateTextureOnly() {
      this.updateGarmentTexture();
    },
    async updateGarmentTexture() {
      try {
        if (!this.vectorTextLayer || !this.vectorTextLayer._canvas) {
          console.warn("Vector text layer or canvas not ready");
          return;
        }

        const dataUrl = this.vectorTextLayer._canvas.toDataURL("image/png");
        this.debouncedApplyTexture(dataUrl);
      } catch (error) {
        console.error("Error applying SVG viewport texture:", error);
      }
    },
    async renderEffectAndApplyTexture() {
      if (!this.vectorTextLayer) return;

      try {
        this.vectorTextLayer.setEffect(this.currentEffect);
        await new Promise((resolve) => setTimeout(resolve, 100));
        this.updateGarmentTexture();
      } catch (error) {
        console.error("Error rendering effect:", error);
      }
    },
    updateLastTextShape(property, value) {
      console.log(`Text shape property changed: ${property} = ${value}`);
    },
    updateSvgEditorOutline() {
      if (!this.vectorTextLayer) {
        return;
      }

      if (this.svgOutlineEnabled) {
        this.vectorTextLayer.addOutline();
      } else {
        this.vectorTextLayer.removeOutline();
      }
    },
  },
};
</script>

<style lang="css">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

.header {
  position: absolute;
  z-index: 999;
  top: 0%;
  left: 0;
  width: 100%;
  height: auto;
  padding: 23px 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: white;
}

#orbitControls {
  position: absolute;
  z-index: 999;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
}

.bottom-panel {
  position: absolute;
  z-index: 999;
  bottom: 0%;
  left: 0;
  padding: 15px;
  width: 100%;
  height: 320px;
  background-color: white;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
}

.thumb-container {
  width: 100%;
  display: flex;
  justify-content: center;
  margin-bottom: 5px;
}

.thumb {
  height: 4px;
  width: 30px;
  border-radius: 4px;
  background-color: #cccccc;
}

.top-tabs,
.bottom-tabs {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  padding: 10px 0;
  border-bottom: 1px solid #f0f0f0;
}

.tab {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.tab p {
  font-size: 10px;
}

.tab.active {
  color: #0070c8;
}

.tab.active img,
.tab.active path {
  color: #0070c8;
}

.image {
  height: 28px;
  width: 28px;
  background-color: black;
  border-radius: 50%;
}

.tab.active .image {
  background-color: #0070c8;
}

.content {
  margin: 10px 0;
  padding-bottom: 60px;
}

#tab2 {
  text-align: center;
}

.color-container {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  align-self: center;
}

.color {
  height: 24px;
  width: 24px;
  border-radius: 50%;
  background-color: aqua;
  margin-left: 10px;
}

.font-variants {
  text-align: center;
  padding: 7px;
  border-bottom: 1px solid #f0f0f0;
  cursor: pointer;
  transition: background-color 0.2s;
}

.font-variants:hover {
  background-color: #f0f0f0;
}

.font-variants.active-font {
  background-color: #e3f2fd;
  border-left: 3px solid #0070c8;
}

.font-variants h4 {
  font-size: 18px;
}

.font-variants p {
  font-size: 12px;
}

.outline-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.outline-header p {
  font-size: 14px;
}

.outline-header div {
  display: flex;
  align-items: center;
  gap: 7px;
}

.outline-footer {
  margin-top: 24px;
}

.outline-footer p {
  font-size: 14px;
}

.outline-footer div {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 7px;
  margin-top: 8px;
}

.outline-footer div input {
  flex: 1;
}

.style-content {
  display: flex;
  flex-wrap: wrap;
}

.style-content .fontStyle {
  width: 25%;
  text-align: center;
  padding: 5px;
  margin-bottom: 10px;
  cursor: pointer;
  border: 2px solid transparent;
  border-radius: 4px;
  transition: all 0.2s;
  display: flex;
  justify-content: center;
  align-items: center;
}

.style-content .fontStyle:hover {
  background-color: #f0f0f0;
}

.style-content .fontStyle.active-shape {
  background-color: #e3f2fd;
  border-color: #0070c8;
}

.style-footer {
  margin-top: 10px;
  text-align: center;
}

.style-footer p {
  font-size: 14px;
  margin-bottom: 4px;
}

.style-footer input {
  width: 100%;
}

.display-none {
  display: none;
}

.styled-range {
  -webkit-appearance: none;
  appearance: none;
  width: 250px;
  background: transparent;
}

/* ===== Chrome / Safari / Edge ===== */

.styled-range::-webkit-slider-runnable-track {
  height: 2px;
  background: #000;
  /* gray track */
}

.styled-range::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 16px;
  height: 16px;
  background: #aaa;
  /* gray thumb */
  border: 1px solid #000;
  /* black border */
  border-radius: 50%;
  margin-top: -8px;
  /* centers thumb on 2px track */
  cursor: pointer;
}

/* ===== Firefox ===== */

.styled-range::-moz-range-track {
  height: 2px;
  background: #000;
}

.styled-range::-moz-range-thumb {
  width: 16px;
  height: 16px;
  background: #aaa;
  border: 1px solid #000;
  border-radius: 50%;
  cursor: pointer;
}

/* Remove Firefox inner hit area padding */
.styled-range::-moz-range-progress {
  background: #000;
  height: 2px;
}

/* ===== Old Edge / IE (optional) ===== */

.styled-range::-ms-track {
  height: 2px;
  background: transparent;
  border-color: transparent;
  color: transparent;
}

.styled-range::-ms-fill-lower,
.styled-range::-ms-fill-upper {
  background: #000;
}

.styled-range::-ms-thumb {
  width: 16px;
  height: 16px;
  background: #aaa;
  border: 1px solid #000;
  border-radius: 50%;
  cursor: pointer;
}

.wrapViewContainer {
  position: relative;
  z-index: 998;
}
</style>
