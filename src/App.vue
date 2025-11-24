<template>
  <div class="header">
    <div><img src="/icons/Arrow.png" alt="" srcset="" /></div>
    <div>
      <img src="/icons/Logo.png" alt="" srcset="" />
    </div>
    <div></div>
  </div>
  <Wrapview ref="wrapView" @onInitalized="environmentMounted"></Wrapview>
  <div id="orbitControls"></div>
  <section class="bottom-panel">
    <div class="svg-preview-panel">
      <h4>SVG Preview</h4>
      <div style="display: flex; align-items: center; gap: 10px">
        <canvas id="svgPreviewCanvas" width="120" height="120" style="border: 1px solid #ccc"></canvas>
        <div style="display: flex; flex-direction: column; gap: 5px">
          <label>Text Color: <input type="color" v-model="svgTextColor" /></label>
          <label>Font Size:
            <input type="number" v-model="svgFontSize" min="8" max="72" /></label>
          <label>Decoration:
            <select v-model="svgTextDecoration">
              <option value="">None</option>
              <option value="bold">Bold</option>
              <option value="italic">Italic</option>
              <option value="bold italic">Bold Italic</option>
            </select>
          </label>
          <button @click="addSvgLayer" style="
              margin-top: 10px;
              padding: 8px 12px;
              background-color: #0070c8;
              color: white;
              border: none;
              border-radius: 4px;
              cursor: pointer;
              width: 100%;
            ">
            Apply to 3D Model
          </button>
        </div>
      </div>
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
      <div class="tab" :class="{ active: activeTab === 0 }" v-on:click="changeTab(0)">
        <img src="/icons/Edit.svg" />
        <p>Edit Text</p>
      </div>
      <div class="tab" :class="{ active: activeTab === 1 }" v-on:click="changeTab(1)">
        <img src="/icons/EditColor.svg" style="height: 24px; width: 24px" />
        <p>Edit Color</p>
      </div>
      <div class="tab" :class="{ active: activeTab === 2 }" v-on:click="changeTab(2)">
        <img src="/icons/Aa.svg" />
        <p>Font</p>
      </div>
      <div class="tab" :class="{ active: activeTab === 3 }" v-on:click="changeTab(3)">
        <img src="/icons/AOutline.svg" />
        <p>Outline</p>
      </div>
      <div class="tab" :class="{ active: activeTab === 4 }" v-on:click="changeTab(4)">
        <img src="/icons/Arch.svg" />
        <p>Shape</p>
      </div>
    </div>
    <div class="content">
      <div id="tab1" :class="{ 'display-none': activeTab !== 0 }">
        <div style="padding: 10px">
          <label style="display: block; margin-bottom: 10px">
            <span style="display: block; margin-bottom: 5px; font-weight: bold">Edit Text</span>
            <input type="text" v-model="svgTextValue" placeholder="Enter text" style="
                width: 100%;
                padding: 8px;
                border: 1px solid #ccc;
                border-radius: 4px;
              " />
          </label>
          <label style="display: block">
            <span style="display: block; margin-bottom: 5px; font-weight: bold">Text Size: {{ svgFontSize }}px</span>
            <input type="range" v-model="svgFontSize" min="8" max="72" style="width: 100%" />
          </label>
        </div>
      </div>
      <div id="tab2" :class="{ 'display-none': activeTab !== 1 }">
        <div class="color-container">
          <div class="color" style="background-color: #000000" @click="svgTextColor = '#000000'" title="Black"></div>
          <div class="color" style="background-color: #ffffff; border: 1px solid #ccc" @click="svgTextColor = '#FFFFFF'"
            title="White"></div>
          <div class="color" style="background-color: #808080" @click="svgTextColor = '#808080'" title="Gray"></div>
          <div class="color" style="background-color: #c0c0c0" @click="svgTextColor = '#C0C0C0'" title="Silver"></div>
          <div class="color" style="background-color: #ff0000" @click="svgTextColor = '#FF0000'" title="Red"></div>
          <div class="color" style="background-color: #8b0000" @click="svgTextColor = '#8B0000'" title="Dark Red"></div>
          <div class="color" style="background-color: #ffa500" @click="svgTextColor = '#FFA500'" title="Orange"></div>
          <div class="color" style="background-color: #ff8c00" @click="svgTextColor = '#FF8C00'" title="Dark Orange">
          </div>
          <div class="color" style="background-color: #ffff00" @click="svgTextColor = '#FFFF00'" title="Yellow"></div>
          <div class="color" style="background-color: #ffd700" @click="svgTextColor = '#FFD700'" title="Gold"></div>
          <div class="color" style="background-color: #00ff00" @click="svgTextColor = '#00FF00'" title="Lime"></div>
          <div class="color" style="background-color: #008000" @click="svgTextColor = '#008000'" title="Green"></div>
          <div class="color" style="background-color: #006400" @click="svgTextColor = '#006400'" title="Dark Green">
          </div>
          <div class="color" style="background-color: #00ffff" @click="svgTextColor = '#00FFFF'" title="Cyan"></div>
          <div class="color" style="background-color: #008b8b" @click="svgTextColor = '#008B8B'" title="Dark Cyan">
          </div>
          <div class="color" style="background-color: #0000ff" @click="svgTextColor = '#0000FF'" title="Blue"></div>
          <div class="color" style="background-color: #000080" @click="svgTextColor = '#000080'" title="Navy"></div>
          <div class="color" style="background-color: #4169e1" @click="svgTextColor = '#4169E1'" title="Royal Blue">
          </div>
          <div class="color" style="background-color: #800080" @click="svgTextColor = '#800080'" title="Purple"></div>
          <div class="color" style="background-color: #4b0082" @click="svgTextColor = '#4B0082'" title="Indigo"></div>
          <div class="color" style="background-color: #ff00ff" @click="svgTextColor = '#FF00FF'" title="Magenta"></div>
          <div class="color" style="background-color: #ff1493" @click="svgTextColor = '#FF1493'" title="Deep Pink">
          </div>
          <div class="color" style="background-color: #ffc0cb" @click="svgTextColor = '#FFC0CB'" title="Pink"></div>
          <div class="color" style="background-color: #a52a2a" @click="svgTextColor = '#A52A2A'" title="Brown"></div>
          <div class="color" style="background-color: #d2691e" @click="svgTextColor = '#D2691E'" title="Chocolate">
          </div>
          <div class="color" style="background-color: #f5f5dc" @click="svgTextColor = '#F5F5DC'" title="Beige"></div>
        </div>
      </div>
      <div id="tab3" :class="{ 'display-none': activeTab !== 2 }">
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Arial' }"
          @click="svgFontFamily = 'Arial'">
          <h4>Warriors</h4>
          <p>Arial</p>
        </div>
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Verdana' }"
          @click="svgFontFamily = 'Verdana'">
          <h4>Warriors</h4>
          <p>Verdana</p>
        </div>
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Times New Roman' }"
          @click="svgFontFamily = 'Times New Roman'">
          <h4>Warriors</h4>
          <p>Times New Roman</p>
        </div>
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Courier New' }"
          @click="svgFontFamily = 'Courier New'">
          <h4>Warriors</h4>
          <p>Courier New</p>
        </div>
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Georgia' }"
          @click="svgFontFamily = 'Georgia'">
          <h4>Warriors</h4>
          <p>Georgia</p>
        </div>
      </div>
      <div id="tab4" :class="{ 'display-none': activeTab !== 3 }">
        <div class="outline-header">
          <div>
            <p>Outline Color</p>
            <div class="color"></div>
          </div>
          <p>5pt</p>
        </div>
        <div class="color-container">
          <div class="color"></div>
          <div class="color"></div>
          <div class="color"></div>
          <div class="color"></div>
          <div class="color"></div>
          <div class="color"></div>
          <div class="color"></div>
        </div>
        <div class="outline-footer">
          <p>Outline Thickness</p>
          <div>
            <p>None</p>
            <input type="range" />
            <p>Very Thick</p>
          </div>
        </div>
      </div>
      <div id="tab5" :class="{ 'display-none': activeTab !== 4 }">
        <div class="style-content">
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'none' }" @click="svgTextShape = 'none'">
            <h2>None</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'arch' }" @click="svgTextShape = 'arch'">
            <h2>Arch</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'bridge' }"
            @click="svgTextShape = 'bridge'">
            <h2>Bridge</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'bulge' }" @click="svgTextShape = 'bulge'">
            <h2>Bulge</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'flag' }" @click="svgTextShape = 'flag'">
            <h2>Flag</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'wave' }" @click="svgTextShape = 'wave'">
            <h2>Wave</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'angle' }" @click="svgTextShape = 'angle'">
            <h2>Angle</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'circle' }"
            @click="svgTextShape = 'circle'">
            <h2>Circle</h2>
          </div>
        </div>
        <div class="style-footer">
          <p>Shape Intensity: {{ svgShapeIntensity }}%</p>
          <input type="range" v-model="svgShapeIntensity" min="0" max="100" />
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import {
  Wrapview,
  Wizard,
  WrapviewInputControl,
} from "@etlok-systems/wrapview-vue";
import {
  OrbitControls,
  WrapviewSettings,
  WrapviewObject,
  WrapviewMaterialSet,
  WrapviewShadowMaterial,
  WrapviewTexturedMaterial,
  WrapviewStitchMaterial,
  WrapviewLight,
  WrapviewTextLayer,
  WrapviewParameter,
  WrapviewUtils,
  WrapviewFontSet,
  WrapviewSvgLayer,
} from "@etlok-systems/wrapview";

export default {
  components: { Wrapview, WrapviewInputControl },
  data() {
    return {
      materials: null,
      size: {
        height: 0,
        width: 0,
      },
      activeTab: 0,
      svgShapes: [],
      svgTextColor: "#000000",
      svgFontSize: 24,
      svgTextDecoration: "",
      svgFontFamily: "Arial",
      svgTextValue: "Demo",
      svgTextShape: "none",
      svgShapeIntensity: 50,
      svgInitialized: false,
      currentSvgLayer: null,
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.initializeSvgText();
    });
  },
  watch: {
    svgTextColor(val) {
      this.updateLastTextShape("fill", val);
    },
    svgFontSize(val) {
      this.updateLastTextShape("fontSize", val);
    },
    svgTextDecoration(val) {
      let fontWeight = "";
      let fontStyle = "";
      if (val === "bold") fontWeight = "bold";
      if (val === "italic") fontStyle = "italic";
      if (val === "bold italic") {
        fontWeight = "bold";
        fontStyle = "italic";
      }
      this.updateLastTextShape("fontWeight", fontWeight);
      this.updateLastTextShape("fontStyle", fontStyle);
    },
    svgFontFamily(val) {
      this.updateLastTextShape("fontFamily", val);
    },
    svgTextValue(val) {
      this.updateLastTextShape("value", val);
    },
    svgTextShape(val) {
      this.updateLastTextShape("textShape", val);
    },
    svgShapeIntensity(val) {
      this.updateLastTextShape("shapeIntensity", val);
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

              this.$refs["wrapView"].instance().animate();

              this.addSvgLayer();
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
        orbitController.enablePan = false;
        orbitController.enableZoom = true;
        orbitController.enableDamping = true;
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
        const dirLight = new WrapviewLight({
          type: "directional",
          color: 0xffffff,
          intensity: 1,
          position: { x: 1, y: 1, z: -2 },
          target: { x: 0, y: 0, z: 0 },
        });

        const hemLight = new WrapviewLight({
          type: "hemisphere",
          color: 0xffffff,
          intensity: 1,
        });

        this.$refs["wrapView"]
          .instance()
          .scene()
          .add(hemLight.createLight(), dirLight.createLight());

        const envPaths = [
          "environment/px.jpg",
          "environment/nx.jpg",
          "environment/py.jpg",
          "environment/ny.jpg",
          "environment/pz.jpg",
          "environment/nz.jpg",
        ];

        const envLight = new WrapviewLight({ type: "ambient", intensity: 1 });
        this.$refs["wrapView"].instance().scene().add(envLight.createLight());

        envLight
          .loadEnvironmentMap(envPaths)
          .then((texture) => {
            this.$refs["wrapView"].instance().scene().environment = texture;
          })
          .catch((err) =>
            console.error("Failed to load environment map:", err)
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

        const collar = new WrapviewTexturedMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              diffuse: "./3001C_SMALL/textures/F_3001C_SMALL_diffuse_1005.png",
              normal: "./3001C_SMALL/textures/F_3001C_SMALL_normal_1005.png",
              alpha: "./3001C_SMALL/textures/F_3001C_SMALL_opacity_1005.png",
              roughness:
                "./3001C_SMALL/textures/F_3001C_SMALL_roughness_1005.png",
              metalness:
                "./3001C_SMALL/textures/F_3001C_SMALL_metalness_1005.png",
            },
          }
        );

        const backNeckTape = new WrapviewTexturedMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              diffuse: "./3001C_SMALL/textures/F_3001C_SMALL_diffuse_1006.png",
              normal: "./3001C_SMALL/textures/F_3001C_SMALL_normal_1006.png",
              alpha: "./3001C_SMALL/textures/F_3001C_SMALL_opacity_1006.png",
              roughness:
                "./3001C_SMALL/textures/F_3001C_SMALL_roughness_1006.png",
              metalness:
                "./3001C_SMALL/textures/F_3001C_SMALL_metalness_1006.png",
            },
          }
        );

        const leftArmSleeve = new WrapviewTexturedMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              diffuse: "./3001C_SMALL/textures/F_3001C_SMALL_diffuse_1003.png",
              normal: "./3001C_SMALL/textures/F_3001C_SMALL_normal_1003.png",
              alpha: "./3001C_SMALL/textures/F_3001C_SMALL_opacity_1003.png",
              roughness:
                "./3001C_SMALL/textures/F_3001C_SMALL_roughness_1003.png",
              metalness:
                "./3001C_SMALL/textures/F_3001C_SMALL_metalness_1003.png",
            },
          }
        );

        const rightArmSleeve = new WrapviewTexturedMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              diffuse: "./3001C_SMALL/textures/F_3001C_SMALL_diffuse_1004.png",
              normal: "./3001C_SMALL/textures/F_3001C_SMALL_normal_1004.png",
              alpha: "./3001C_SMALL/textures/F_3001C_SMALL_opacity_1004.png",
              roughness:
                "./3001C_SMALL/textures/F_3001C_SMALL_roughness_1004.png",
              metalness:
                "./3001C_SMALL/textures/F_3001C_SMALL_metalness_1004.png",
            },
          }
        );

        const frontBody = new WrapviewTexturedMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              base: "./3001C_SMALL/textures/F_3001C_SMALL_common.png", // Base layer for text editing
              diffuse: "./3001C_SMALL/textures/F_3001C_SMALL_common.png",
              normal: "./3001C_SMALL/textures/F_3001C_SMALL_normal_1001.png",
              alpha: "./3001C_SMALL/textures/F_3001C_SMALL_opacity_1001.png",
              roughness:
                "./3001C_SMALL/textures/F_3001C_SMALL_roughness_1001.png",
              metalness:
                "./3001C_SMALL/textures/F_3001C_SMALL_metalness_1001.png",
            },
            build: {
              parameters: {
                base: true, // Enable base layer building for text editing
                size: 2048,
              },
            },
          }
        );

        const backBody = new WrapviewTexturedMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              diffuse: "./3001C_SMALL/textures/F_3001C_SMALL_common.png",
              normal: "./3001C_SMALL/textures/F_3001C_SMALL_normal_1002.png",
              alpha: "./3001C_SMALL/textures/F_3001C_SMALL_opacity_1002.png",
              roughness:
                "./3001C_SMALL/textures/F_3001C_SMALL_roughness_1002.png",
              metalness:
                "./3001C_SMALL/textures/F_3001C_SMALL_metalness_1002.png",
            },
          }
        );

        const stitches = new WrapviewStitchMaterial(
          this.$refs["wrapView"].instance(),
          {
            resources: {
              diffuse: "./3001C_SMALL/textures/Basic_Offset_2193.png",
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
          stitches.init()
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
              y: 0.13,
            },
            scale: {
              x: 0.8,
              y: 0.8,
              z: 0.8,
            },
          },
        });
        item.setMaterials(materials);
        item.load("./3001C_SMALL/3001C_SMALL_LOD0.glb").then(() => {
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
      console.log(panel);
      return panel;
    },
    addTextLayer() {
      const panel = this.currentPanel();
      if (!panel) {
        console.error("Cannot add text layer: panel not found");
        return;
      }

      if (!panel.texture()) {
        console.error("Cannot add text layer: panel texture not initialized");
        return;
      }

      var fonts = new WrapviewFontSet();
      fonts.load([
        {
          name: "Roboto",
          source: "google",
          id: "1",
          value: function (n) {
            return this[n];
          },
        },
      ]);

      var color = new WrapviewParameter(panel, "textColor");
      color.set({
        type: "fixed",
        value: "#000000",
        descriptor: "Black",
      });

      var outline = new WrapviewParameter(panel, "outlineColor");
      outline.set({
        type: "fixed",
        value: "#000000",
        descriptor: "Black",
      });

      var size = panel.settings.build.parameters.size;
      const textLayer = new WrapviewTextLayer(WrapviewUtils.guid(), {
        pivot: {
          x: 0.5,
          y: 0.5,
        },
        position: {
          x: size / 2,
          y: size / 2,
        },
        angle: 0,
        fontSize: 70,
        font: fonts.first(),
        color: color,
        outline: {
          include: false,
          color: outline,
          thickness: 1,
        },
      });

      // Begin editing the texture before adding layers
      panel
        .texture()
        .beginEditing()
        .then(() => {
          var layerIndex = panel.texture().addLayer(textLayer);
          textLayer
            .load(
              {
                text: {
                  type: "fixed",
                  value: "Text",
                },
              },
              panel
            )
            .then(() => {
              panel.texture().editLayer(layerIndex);
              panel.texture().render();
              // panel.texture().endEditing();
            })
            .catch((err) => {
              console.error("Error loading text layer:", err);
            });
        })
        .catch((err) => {
          console.error("Error in beginEditing():", err);
        });
    },

    initializeSvgText() {
      if (this.svgInitialized) return;
      let fontWeight = "";
      let fontStyle = "";
      if (this.svgTextDecoration === "bold") fontWeight = "bold";
      if (this.svgTextDecoration === "italic") fontStyle = "italic";
      if (this.svgTextDecoration === "bold italic") {
        fontWeight = "bold";
        fontStyle = "italic";
      }

      this.svgShapes.push({
        type: "text",
        value: this.svgTextValue,
        x: 60,
        y: 60,
        fontSize: this.svgFontSize,
        fill: this.svgTextColor,
        fontFamily: this.svgFontFamily,
        fontWeight,
        fontStyle,
        textShape: this.svgTextShape,
        shapeIntensity: this.svgShapeIntensity,
      });
      this.svgInitialized = true;
      this.updateSvgPreview();
    },

    updateSvgPreview() {
      var svgData = this.buildSvgData();
      var canvas = document.getElementById("svgPreviewCanvas");
      if (!canvas) return;
      var ctx = canvas.getContext("2d");
      var DOMURL = window.URL || window.webkitURL || window;
      var img = new window.Image();
      var svg = new Blob([svgData], { type: "image/svg+xml;charset=utf-8" });
      var url = DOMURL.createObjectURL(svg);
      img.onload = function () {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.drawImage(img, 0, 0);
        DOMURL.revokeObjectURL(url);
      };
      img.src = url;
    },
    buildSvgData() {
      var svg = `<svg xmlns='http://www.w3.org/2000/svg' width='120' height='120'>`;
      for (var i = 0; i < this.svgShapes.length; i++) {
        var shape = this.svgShapes[i];
        if (shape.type === "text") {
          var style = `font-size:${shape.fontSize}px; fill:${shape.fill}; font-family:${shape.fontFamily};`;
          if (shape.fontWeight) style += ` font-weight:${shape.fontWeight};`;
          if (shape.fontStyle) style += ` font-style:${shape.fontStyle};`;

          if (shape.textShape && shape.textShape !== "none") {
            // Apply text path transformation for shapes
            var pathData = this.getTextPathForShape(
              shape.textShape,
              shape.shapeIntensity || 50
            );
            svg += `<defs><path id='textPath${i}' d='${pathData}' fill='none'/></defs>`;
            svg += `<text style='${style}'><textPath href='#textPath${i}' startOffset='50%' text-anchor='middle'>${shape.value}</textPath></text>`;
          } else {
            svg += `<text x='${shape.x}' y='${shape.y}' text-anchor='middle' dominant-baseline='middle' style='${style}'>${shape.value}</text>`;
          }
        }
      }
      svg += `</svg>`;
      return svg;
    },

    getTextPathForShape(shapeType, intensity) {
      const width = 120;
      const height = 120;
      const centerY = 60;
      const startX = 20;
      const endX = 100;
      const curve = (intensity / 100) * 30; // Scale intensity to pixel offset

      switch (shapeType) {
        case "arch":
          return `M ${startX} ${centerY} Q ${width / 2} ${centerY - curve
            } ${endX} ${centerY}`;
        case "bridge":
          return `M ${startX} ${centerY} Q ${width / 2} ${centerY + curve
            } ${endX} ${centerY}`;
        case "bulge":
          return `M ${startX} ${centerY} Q ${width / 2} ${centerY + curve
            } ${endX} ${centerY}`;
        case "flag":
          return `M ${startX} ${centerY} Q ${width / 3} ${centerY - curve} ${width / 2
            } ${centerY} Q ${(2 * width) / 3} ${centerY + curve
            } ${endX} ${centerY}`;
        case "wave":
          return `M ${startX} ${centerY} Q ${width / 3} ${centerY + curve} ${width / 2
            } ${centerY} Q ${(2 * width) / 3} ${centerY - curve
            } ${endX} ${centerY}`;
        case "angle":
          return `M ${startX} ${centerY + curve / 2} L ${width / 2} ${centerY - curve / 2
            } L ${endX} ${centerY + curve / 2}`;
        case "circle":
          const radius = 30 + (intensity / 100) * 10;
          return `M ${width / 2 - radius
            } ${centerY} A ${radius} ${radius} 0 0 1 ${width / 2 + radius
            } ${centerY}`;
        default:
          return `M ${startX} ${centerY} L ${endX} ${centerY}`;
      }
    },
    updateLastTextShape(key, value) {
      if (!this.svgShapes.length) return;
      const last = this.svgShapes[this.svgShapes.length - 1];
      if (last.type === "text") {
        last[key] = value;
        this.updateSvgPreview();
      }
    },

    addSvgLayer() {
      const panel = this.currentPanel();
      if (!panel) {
        console.error("Cannot add SVG layer: panel not found");
        return;
      }

      if (!panel.texture()) {
        console.error("Cannot add SVG layer: panel texture not initialized");
        return;
      }

      var size = panel.settings.build.parameters.size;

      // Create or update SVG layer
      if (!this.currentSvgLayer) {
        this.currentSvgLayer = new WrapviewSvgLayer(WrapviewUtils.guid(), {
          size: { width: size, height: size },
          pivot: { x: 0.5, y: 0.5 },
          position: { x: size / 2, y: size / 2 },
          angle: 0,
        });
      }

      // Begin editing the texture before adding layers
      panel
        .texture()
        .beginEditing()
        .then(() => {
          // Build SVG data from current state - scale it up to material size
          const svgData = this.buildSvgDataForMaterial(size);

          // Check if SVG layer already exists
          const layers = panel.texture().layers();
          let layerIndex = panel.texture().addLayer(this.currentSvgLayer);
          for (let i = 0; i < layers.length; i++) {
            if (layers[i].id === this.currentSvgLayer.id) {
              layerIndex = i;
              break;
            }
          }

          // Load SVG data into the layer
          this.currentSvgLayer
            .load({ svgData: svgData })
            .then(() => {
              panel.texture().editLayer(layerIndex);
              panel.texture().render();
              // panel.texture().endEditing();
            })
            .catch((err) => {
              console.error("Error loading SVG layer:", err);
            });
        })
        .catch((err) => {
          console.error("Error in beginEditing():", err);
        });
    },

    buildSvgDataForMaterial(size) {
      // Build SVG with scaled dimensions for the material
      var svg = `<svg xmlns='http://www.w3.org/2000/svg' width='${size}' height='${size}'>`;

      for (var i = 0; i < this.svgShapes.length; i++) {
        var shape = this.svgShapes[i];
        if (shape.type === "text") {
          // Scale positions and font size for material
          const scale = size / 120; // 120 is preview canvas size
          const scaledX = shape.x * scale;
          const scaledY = shape.y * scale;
          const scaledFontSize = shape.fontSize * scale;

          var style = `font-size:${scaledFontSize}px; fill:${shape.fill}; font-family:${shape.fontFamily};`;
          if (shape.fontWeight) style += ` font-weight:${shape.fontWeight};`;
          if (shape.fontStyle) style += ` font-style:${shape.fontStyle};`;

          if (shape.textShape && shape.textShape !== "none") {
            // Apply text path transformation for shapes (scaled)
            var pathData = this.getTextPathForShapeScaled(
              shape.textShape,
              shape.shapeIntensity || 50,
              size
            );
            svg += `<defs><path id='textPath${i}' d='${pathData}' fill='none'/></defs>`;
            svg += `<text style='${style}'><textPath href='#textPath${i}' startOffset='50%' text-anchor='middle'>${shape.value}</textPath></text>`;
          } else {
            svg += `<text x='${scaledX}' y='${scaledY}' text-anchor='middle' dominant-baseline='middle' style='${style}'>${shape.value}</text>`;
          }
        }
      }

      svg += `</svg>`;
      return svg;
    },

    getTextPathForShapeScaled(shapeType, intensity, size) {
      // Generate path for material size
      const centerY = size / 2;
      const startX = size * 0.167; // ~20/120
      const endX = size * 0.833; // ~100/120
      const curve = (intensity / 100) * (size * 0.25); // Scale curve with size

      switch (shapeType) {
        case "arch":
          return `M ${startX} ${centerY} Q ${size / 2} ${centerY - curve
            } ${endX} ${centerY}`;
        case "bridge":
          return `M ${startX} ${centerY} Q ${size / 2} ${centerY + curve
            } ${endX} ${centerY}`;
        case "bulge":
          return `M ${startX} ${centerY} Q ${size / 2} ${centerY + curve
            } ${endX} ${centerY}`;
        case "flag":
          return `M ${startX} ${centerY} Q ${size / 3} ${centerY - curve} ${size / 2
            } ${centerY} Q ${(2 * size) / 3} ${centerY + curve
            } ${endX} ${centerY}`;
        case "wave":
          return `M ${startX} ${centerY} Q ${size / 3} ${centerY + curve} ${size / 2
            } ${centerY} Q ${(2 * size) / 3} ${centerY - curve
            } ${endX} ${centerY}`;
        case "angle":
          return `M ${startX} ${centerY + curve / 2} L ${size / 2} ${centerY - curve / 2
            } L ${endX} ${centerY + curve / 2}`;
        case "circle":
          const radius = size * 0.25 + (intensity / 100) * (size * 0.083);
          return `M ${size / 2 - radius
            } ${centerY} A ${radius} ${radius} 0 0 1 ${size / 2 + radius
            } ${centerY}`;
        default:
          return `M ${startX} ${centerY} L ${endX} ${centerY}`;
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
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
}

.bottom-panel {
  position: absolute;
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
  margin-top: 10px;
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
  height: 28px;
  width: 28px;
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
</style>
