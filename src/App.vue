<template>
  <div class="header">
    <div><img src="/icons/Arrow.png" alt="" srcset="" /></div>
    <div>
      <img src="/icons/Logo.png" alt="" srcset="" />
    </div>
    <div></div>
  </div>
  <Wrapview class="wrapViewContainer" ref="wrapView" @onInitalized="environmentMounted"></Wrapview>
  <div id="orbitControls"></div>
  <section class="bottom-panel">
    <div class="svg-preview-panel" style="display: none;">
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
          <button @click="addSvgLayer" style="margin-top: 10px; padding: 8px 12px; background-color: #0070c8; color: white; border: none; border-radius: 4px; cursor: pointer; width: 100%;">
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
        <div style="padding: 10px; display: flex; flex-direction: column; flex-wrap: wrap; gap: 15px;">
          <label style="flex: 1;">
            <span style="display: block; margin-bottom: 5px; font-weight: bold">Edit Text</span>
            <input type="text" v-model="svgTextValue" placeholder="Enter text" style="width: 100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px;" />
          </label>
          <label style="flex: 1; width: 100%;">
            <span style="display: block; margin-bottom: 5px; font-weight: bold">Text Size (px)</span>
            <input type="range" v-model="svgFontSize" min="0" max="50" class="styled-range w-full" />
          </label>
        </div>
      </div>
      <div id="tab2" :class="{ 'display-none': activeTab !== 1 }">
        <div class="color-container">
          <div class="color" style="background-color: #FFFFFF; border: 1px solid #DDDDDD" @click="svgTextColor = '#FFFFFF'" title="White"></div>
          <div class="color" style="background-color: #000000" @click="svgTextColor = '#000000'" title="Black"></div>
          <div class="color" style="background: linear-gradient(45deg, #ccc 25%, transparent 25%, transparent 75%, #ccc 75%, #ccc), linear-gradient(45deg, #ccc 25%, transparent 25%, transparent 75%, #ccc 75%, #ccc); background-size: 8px 8px; background-position: 0 0, 4px 4px; border: 1px solid #DDDDDD" @click="svgTextColor = 'transparent'" title="Transparent"></div>
          <div class="color" style="background-color: #595F5E" @click="svgTextColor = '#595F5E'" title="Dark Gray"></div>
          <div class="color" style="background-color: #FF0086" @click="svgTextColor = '#FF0086'" title="Hot Pink"></div>
          <div class="color" style="background-color: #FFA5B6" @click="svgTextColor = '#FFA5B6'" title="Light Pink"></div>
          <div class="color" style="background-color: #FF80B6" @click="svgTextColor = '#FF80B6'" title="Medium Pink"></div>
          <div class="color" style="background-color: #AD3D80" @click="svgTextColor = '#AD3D80'" title="Purple Pink"></div>
          <div class="color" style="background-color: #732735" @click="svgTextColor = '#732735'" title="Maroon"></div>
          <div class="color" style="background-color: #C30C27" @click="svgTextColor = '#C30C27'" title="Red"></div>
          <div class="color" style="background-color: #EF2B3B" @click="svgTextColor = '#EF2B3B'" title="Bright Red"></div>
          <div class="color" style="background-color: #FF761C" @click="svgTextColor = '#FF761C'" title="Orange"></div>
          <div class="color" style="background-color: #FFAB29" @click="svgTextColor = '#FFAB29'" title="Light Orange"></div>
          <div class="color" style="background-color: #FFC032" @click="svgTextColor = '#FFC032'" title="Gold Orange"></div>
          <div class="color" style="background-color: #FFD725" @click="svgTextColor = '#FFD725'" title="Yellow"></div>
          <div class="color" style="background-color: #F9E85C" @click="svgTextColor = '#F9E85C'" title="Light Yellow"></div>
          <div class="color" style="background-color: #815E24" @click="svgTextColor = '#815E24'" title="Brown"></div>
          <div class="color" style="background-color: #60C8A9" @click="svgTextColor = '#60C8A9'" title="Mint"></div>
          <div class="color" style="background-color: #98DD22" @click="svgTextColor = '#98DD22'" title="Lime Green"></div>
          <div class="color" style="background-color: #289B24" @click="svgTextColor = '#289B24'" title="Green"></div>
          <div class="color" style="background-color: #005E31" @click="svgTextColor = '#005E31'" title="Dark Green"></div>
          <div class="color" style="background-color: #27472D" @click="svgTextColor = '#27472D'" title="Forest Green"></div>
          <div class="color" style="background-color: #00686C" @click="svgTextColor = '#00686C'" title="Teal"></div>
          <div class="color" style="background-color: #0091AA" @click="svgTextColor = '#0091AA'" title="Cyan"></div>
          <div class="color" style="background-color: #1DA6EB" @click="svgTextColor = '#1DA6EB'" title="Sky Blue"></div>
          <div class="color" style="background-color: #86B6E6" @click="svgTextColor = '#86B6E6'" title="Light Blue"></div>
          <div class="color" style="background-color: #045594" @click="svgTextColor = '#045594'" title="Blue"></div>
          <div class="color" style="background-color: #0E3EB0" @click="svgTextColor = '#0E3EB0'" title="Royal Blue"></div>
          <div class="color" style="background-color: #073575" @click="svgTextColor = '#073575'" title="Dark Blue"></div>
          <div class="color" style="background-color: #032C4F" @click="svgTextColor = '#032C4F'" title="Navy"></div>
          <div class="color" style="background-color: #A271BF" @click="svgTextColor = '#A271BF'" title="Light Purple"></div>
          <div class="color" style="background-color: #B523B1" @click="svgTextColor = '#B523B1'" title="Purple"></div>
        </div>
      </div>
      <div id="tab3" :class="{ 'display-none': activeTab !== 2 }">
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Asap Condensed' }" @click="svgFontFamily = 'Asap Condensed'">
          <h4 style="font-weight: 600;font-family: 'Asap Condensed', sans-serif;">{{ svgTextValue }}</h4>
          <p>Asap Condensed</p>
        </div>
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Baloo 2' }" @click="svgFontFamily = 'Baloo 2'">
          <h4 style="font-weight: 600;font-family: 'Baloo 2', sans-serif;">{{ svgTextValue }}</h4>
          <p>Baloo</p>
        </div>
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Caprasimo' }" @click="svgFontFamily = 'Caprasimo'">
          <h4 style="font-weight: 600;font-family: 'Caprasimo', serif;">{{ svgTextValue }}</h4>
          <p>Caprasimo</p>
        </div>
        <div class="font-variants" :class="{ 'active-font': svgFontFamily === 'Caramel' }" @click="svgFontFamily = 'Caramel'">
          <h4 style="font-weight: 600;font-family: 'Caramel', cursive">{{ svgTextValue }}</h4>
          <p>Caramel</p>
        </div>
      </div>
      <div id="tab4" :class="{ 'display-none': activeTab !== 3 }">
        <div class="outline-header">
          <div>
            <p>Enable Outline</p>
            <input type="checkbox" v-model="svgOutlineEnabled" style="width: 20px; height: 20px; cursor: pointer;" />
          </div>
          <p>{{ svgOutlineThickness }}pt</p>
        </div>
        <div class="color-container">
          <div class="color" style="background-color: #FFFFFF; border: 1px solid #DDDDDD" @click="svgOutlineColor = '#FFFFFF'" title="White"></div>
          <div class="color" style="background-color: #000000" @click="svgOutlineColor = '#000000'" title="Black"></div>
          <div class="color" style="background-color: #595F5E" @click="svgOutlineColor = '#595F5E'" title="Dark Gray"></div>
          <div class="color" style="background-color: #C30C27" @click="svgOutlineColor = '#C30C27'" title="Red"></div>
          <div class="color" style="background-color: #FF761C" @click="svgOutlineColor = '#FF761C'" title="Orange"></div>
          <div class="color" style="background-color: #FFD725" @click="svgOutlineColor = '#FFD725'" title="Yellow"></div>
          <div class="color" style="background-color: #289B24" @click="svgOutlineColor = '#289B24'" title="Green"></div>
          <div class="color" style="background-color: #0091AA" @click="svgOutlineColor = '#0091AA'" title="Cyan"></div>
          <div class="color" style="background-color: #045594" @click="svgOutlineColor = '#045594'" title="Blue"></div>
          <div class="color" style="background-color: #B523B1" @click="svgOutlineColor = '#B523B1'" title="Purple"></div>
          <div class="color" style="background-color: #AD3D80" @click="svgOutlineColor = '#AD3D80'" title="Purple Pink"></div>
        </div>
        <div class="outline-footer" style="display: flex; flex-direction: column; align-items: center; gap: 10px;">
          <p style="margin: 0;">Outline Thickness (pt)</p>
          <input type="range" v-model="svgOutlineThickness" min="0" max="10" class="styled-range flex-1" />
        </div>
      </div>
      <div id="tab5" :class="{ 'display-none': activeTab !== 4 }">
        <div class="style-content">
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'none' }" @click="svgTextShape = 'none'">
            <h2>None</h2>
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'arch' }" @click="svgTextShape = 'arch'">
            <img src="/icons/arch.png" />
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'bulge' }" @click="svgTextShape = 'bulge'">
            <img src="/icons/buldge.png" />
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'flag' }" @click="svgTextShape = 'flag'">
            <img src="/icons/flag.png" />
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'distort' }" @click="svgTextShape = 'distort'">
            <img src="/icons/angle.png" />
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'circle' }" @click="svgTextShape = 'circle'">
            <img src="/icons/circle.png" />
          </div>
          <div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'pinch' }" @click="svgTextShape = 'pinch'">
            <img src="/icons/pinch.png" />
          </div>
        </div>
        <div class="style-footer">
          <p>Shape Intensity: {{ svgShapeIntensity }}%</p>
          <input type="range" v-model="svgShapeIntensity" min="0" max="100" class="styled-range" />
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
import { fabric } from "fabric";

export default {
	// ... components, data, mounted same as before ...
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
			svgTextColor: "#ffffff",
			svgFontSize: 6,
			svgTextDecoration: "",
			svgFontFamily: "Asap Condensed",
			svgTextValue: "WARRIORS",
			svgTextShape: "none",
			svgShapeIntensity: 50,
			svgOutlineColor: "#000000",
			svgOutlineThickness: 1,
			svgOutlineEnabled: false,
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
		svgTextColor(val) { this.syncShape("svgTextColor", val); },
		svgFontSize(val) { this.syncShape("svgFontSize", val); },
		svgFontFamily(val) { this.syncShape("svgFontFamily", val); },
		svgTextValue(val) { this.syncShape("svgTextValue", val); },
		svgTextShape(val) { this.syncShape("svgTextShape", val); },
		svgShapeIntensity(val) { this.syncShape("svgShapeIntensity", val); },
		svgOutlineColor(val) { this.syncShape("svgOutlineColor", val); },
		svgOutlineThickness(val) { this.syncShape("svgOutlineThickness", val); },
		svgOutlineEnabled(val) { this.syncShape("svgOutlineEnabled", val); },
		svgTextDecoration(val) {
			this.updateLastTextShape("fontWeight", this.parseFontWeight());
			this.updateLastTextShape("fontStyle", this.parseFontStyle());
		},
	},

	methods: {
		// ============================================
		// PHASE 1: UTILITY FUNCTION CONSOLIDATION
		// ============================================

		/**
		 * Parse font weight from decoration string
		 * @returns {string} "bold" or ""
		 */
		parseFontWeight() {
			return this.svgTextDecoration.includes("bold") ? "bold" : "";
		},

		/**
		 * Parse font style from decoration string
		 * @returns {string} "italic" or ""
		 */
		parseFontStyle() {
			return this.svgTextDecoration.includes("italic") ? "italic" : "";
		},

		/**
		 * Generic watcher handler for syncing shape properties
		 * Maps component properties to shape object keys
		 */
		syncShape(property, val) {
			const propertyMap = {
				svgTextColor: "fill",
				svgFontSize: "fontSize",
				svgFontFamily: "fontFamily",
				svgTextValue: "value",
				svgTextShape: "textShape",
				svgShapeIntensity: "shapeIntensity",
				svgOutlineColor: "outlineColor",
				svgOutlineThickness: "outlineThickness",
				svgOutlineEnabled: "outlineEnabled",
			};

			const key = propertyMap[property];
			if (key) {
				this.updateLastTextShape(key, val);
			}
		},

		/**
		 * Factory function to create shape objects with defaults
		 * Eliminates hardcoded shape creation logic
		 */
		createShapeObject(overrides = {}) {
			const defaults = {
				type: "text",
				value: this.svgTextValue,
				x: 60,
				y: 60,
				fontSize: this.svgFontSize,
				fill: this.svgTextColor,
				fontFamily: this.svgFontFamily,
				fontWeight: this.parseFontWeight(),
				fontStyle: this.parseFontStyle(),
				textShape: this.svgTextShape,
				shapeIntensity: this.svgShapeIntensity,
				outlineColor: this.svgOutlineColor,
				outlineThickness: this.svgOutlineThickness,
				outlineEnabled: this.svgOutlineEnabled,
			};
			return { ...defaults, ...overrides };
		},

		// ============================================
		// EXISTING CORE FUNCTIONS (unchanged)
		// ============================================

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
				orbitController.enabled = false;
				orbitController.enableZoom = true;
				orbitController.enableDamping = false;
				orbitController.minDistance = 1;
				orbitController.maxDistance = 2;

				this.resetCamera();
				this.$refs["wrapView"].instance().setController(orbitController);
				resolve();
			});
		},

		resetCamera() {
			this.$refs["wrapView"].instance().camera().position.set(0, 0, -1.5);
		},

		loadLights() {
			return new Promise((resolve, reject) => {
				const lightConfigs = [
					{ x: 4, y: 25, z: 9, intensity: 5 },
					{ x: 4, y: 12, z: -8, intensity: 5 },
					{ x: 0, y: 55, z: 0, intensity: 7 },
				];

				const lights = lightConfigs.map(config => {
					const light = new WrapviewLight({
						type: "rectarea",
						color: 0xffffff,
						intensity: config.intensity,
						position: { x: config.x, y: config.y, z: config.z },
						width: 30,
						height: 30
					});
					const createdLight = light.createLight();
					createdLight.lookAt(0, 0, 0);
					return createdLight;
				});

				this.$refs["wrapView"].instance().scene().add(...lights);
				resolve();
			});
		},

		loadMaterials() {
			return new Promise((resolve, reject) => {
				this.$refs["wrapView"].instance().updateOffsets();
				const promises = [];
				var materials = new WrapviewMaterialSet();

				const textColor = new WrapviewParameter(null, "textColor");
				textColor.set({
					type: "fixed",
					value: "#2b2b2b",
					descriptor: "Black",
				});

				// Material configurations - consolidated
				const materialConfigs = {
					COLLAR: {
						diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1005.png",
						normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1005.png",
						alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1005.png",
						metalness: "/3001C_SMALL/textures/F_3001C_SMALL_metalness_1005.png",
					},
					BACK_NECK_TAPE: {
						diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1006.png",
						normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1006.png",
						alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1006.png",
						metalness: "/3001C_SMALL/textures/F_3001C_SMALL_metalness_1006.png",
					},
					LEFT_ARM_SLEEVE: {
						diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1003.png",
						normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1003.png",
						alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1003.png",
						metalness: "/3001C_SMALL/textures/F_3001C_SMALL_metalness_1003.png",
					},
					RIGHT_ARM_SLEEVE: {
						diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_diffuse_1004.png",
						normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1004.png",
						alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1004.png",
						metalness: "/3001C_SMALL/textures/F_3001C_SMALL_metalness_1004.png",
					},
					FRONT_BODY: {
						base: "/3001C_SMALL/textures/F_3001C_SMALL_common.png",
						diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_common.png",
						normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1001.png",
						alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1001.png",
						metalness: "/3001C_SMALL/textures/F_3001C_SMALL_metalness_1001.png",
					},
					BACK_BODY: {
						base: "/3001C_SMALL/textures/F_3001C_SMALL_common.png",
						diffuse: "/3001C_SMALL/textures/F_3001C_SMALL_common.png",
						normal: "/3001C_SMALL/textures/F_3001C_SMALL_normal_1002.png",
						alpha: "/3001C_SMALL/textures/F_3001C_SMALL_opacity_1002.png",
						metalness: "/3001C_SMALL/textures/F_3001C_SMALL_metalness_1002.png",
					},
				};

				// Create textured materials from configs
				Object.entries(materialConfigs).forEach(([name, textures]) => {
					const material = new WrapviewTexturedMaterial(
						this.$refs["wrapView"].instance(),
						{
							resources: {
								base: textures.base || textures.diffuse,
								diffuse: textures.diffuse,
								normal: textures.normal,
								alpha: textures.alpha,
								metalness: textures.metalness,
							},
							build: {
								parameters: {
									base: true,
									size: 2048,
									layers: [],
									color: textColor
								},
							},
						}
					);
					promises.push(material.init());
					materials.add(name, material);
				});

				// Shadow and Stitches materials
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
							diffuse: "/3001C_SMALL/textures/Basic_Offset_2193.png",
						},
					}
				);

				promises.push(shadow.init(), stitches.init());
				materials.add("EXT_Stitches", stitches);
				materials.add("99_ShadowPanel", shadow);

				Promise.all(promises).then(
					() => resolve({ materials }),
					(e) => console.error("Error loading materials:", e)
				);
			});
		},

		loadObjects(materials) {
			return new Promise((resolve, reject) => {
				const item = new WrapviewObject({
					transform: {
						rotation: { y: -Math.PI },
						position: { y: 0.16 },
						scale: { x: 0.8, y: 0.8, z: 0.8 },
					},
				});
				item.setMaterials(materials);
				item.load("/3001C_SMALL/3001C_SMALL_LOD0.glb").then(() => {
					this.$refs["wrapView"].instance().addObject(item);
					resolve();
				});
			});
		},

		currentPanel() {
			const panel = this.materials.get("FRONT_BODY");
			if (!panel) console.error("FRONT_BODY panel not found in materials");
			return panel;
		},

		initializeSvgText() {
			if (this.svgInitialized) return;
			this.svgShapes.push(this.createShapeObject());
			this.svgInitialized = true;
			this.updateSvgPreview();
		},

		// ============================================
		// PHASE 2: UNIFIED SVG RENDERING
		// ============================================

		updateSvgPreview() {
			var svgData = this.buildSvgData();
			var canvas = document.getElementById("svgPreviewCanvas");
			if (!canvas) return;
			// Use high DPI rendering for preview
			const dpr = window.devicePixelRatio || 1;
			canvas.width = 120 * dpr;
			canvas.height = 120 * dpr;
			const ctx = canvas.getContext("2d", { antialias: true, alpha: true });
			ctx.scale(dpr, dpr);
			var DOMURL = window.URL || window.webkitURL || window;
			var img = new window.Image();
			var svg = new Blob([svgData], { type: "image/svg+xml;charset=utf-8" });
			var url = DOMURL.createObjectURL(svg);
			img.onload = function () {
				ctx.clearRect(0, 0, 120, 120);
				ctx.drawImage(img, 0, 0, 120, 120);
				DOMURL.revokeObjectURL(url);
			};
			img.src = url;
		},

		buildSvgData(size = 120) {
			var svg = `<svg xmlns='http://www.w3.org/2000/svg' width='${size}' height='${size}'>`;
			for (var shape of this.svgShapes) {
				if (shape.type === "text") {
					svg += this.createTextElementSVG(shape, size);
				}
			}
			svg += `</svg>`;
			return svg;
		},

		createTextElementSVG(shape, canvasSize) {
			let svg = `<svg xmlns='http://www.w3.org/2000/svg' width='${canvasSize}' height='${canvasSize}'>`;
			svg += `<defs><filter id='previewFilter' x='-50%' y='-50%' width='200%' height='200%'><feGaussianBlur in='SourceGraphic' stdDeviation='0' /></filter></defs>`;
			
			// Render outline first (underneath)
			svg += this.createOutlineGroup(shape.value, shape, canvasSize, 1);

			// Render text on top with anti-aliasing
			const baseStyle = `font-size:${shape.fontSize}px; fill:${shape.fill}; font-family:${shape.fontFamily}; font-weight:${shape.fontWeight || 'normal'}; font-style:${shape.fontStyle || 'normal'}; -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale; text-rendering:geometricPrecision; filter:url(#previewFilter);`;

			if (shape.textShape && shape.textShape !== "none") {
				svg += this.renderShapeText(shape.value, shape, canvasSize, baseStyle, shape.textShape);
			} else {
				const cx = canvasSize / 2;
				const cy = canvasSize / 2;
				svg += `<text x='${cx}' y='${cy}' text-anchor='middle' dominant-baseline='middle' style='${baseStyle}'>${this.escapeXml(shape.value)}</text>`;
			}

			svg += `</svg>`;
			return svg;
		},

		/**
		 * UNIFIED SHAPE RENDERING - Works for both preview and material
		 * Replaces createShapeText() and createShapeTextMaterial()
		 * @param {string} text - Text to render
		 * @param {object} shape - Shape configuration
		 * @param {number} size - Canvas/material size
		 * @param {string} style - CSS style string
		 * @param {string} shapeType - Type of distortion
		 * @param {number} scale - Scale factor (1 for preview, size/120 for material)
		 */
		renderShapeText(text, shape, size, style, shapeType, scale = 1) {
			const chars = text.split('');
			const len = chars.length;
			const mid = (len - 1) / 2;
			const shapeData = this.getTextPathForShape(shapeType, shape.shapeIntensity, size, text);

			// Route to specific shape renderer
			const rendererName = `render${shapeType.charAt(0).toUpperCase() + shapeType.slice(1)}Shape`;
			const renderer = this[rendererName];

			if (!renderer) {
				console.warn(`Renderer not found: ${rendererName}`);
				return "";
			}

			return renderer.call(this, chars, mid, shapeData, size, style, shape, scale);
		},

		// ============================================
		// SHAPE-SPECIFIC RENDERERS
		// ============================================

		renderArchShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const len = chars.length;

			// If intensity is very low (less than 10%), render as straight line
			if (shapeData.intensityFactor < 0.1) {
				const charWidth = size * 0.025;
				const spacing = size * 0.002;
				const totalWidth = chars.length * charWidth + spacing * Math.max(chars.length - 1, 0);
				let currentX = (size - totalWidth) / 2;

				chars.forEach((char) => {
					const posX = currentX + charWidth / 2;
					const posY = size / 2;
					svg += `<text x='${posX}' y='${posY}' text-anchor='middle' dominant-baseline='middle' style='${style}'>${this.escapeXml(char)}</text>`;
					currentX += charWidth + spacing;
				});
			} else {
				// Render as arch with reduced effect at lower intensities
				chars.forEach((char, i) => {
					const angle = shapeData.startAngle + (i / (len - 1 || 1)) * shapeData.angleRange;
					const x = size / 2 + Math.cos(angle) * shapeData.radius;
					const y = size / 2 + Math.sin(angle) * shapeData.radius;
					const rotation = (angle * 180 / Math.PI) + 90;
					svg += `<text x='${x}' y='${y}' text-anchor='middle' dominant-baseline='middle' style='${style}' transform='rotate(${rotation} ${x} ${y})'>${this.escapeXml(char)}</text>`;
				});
			}

			return svg;
		},

		renderBridgeShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const charWidth = size * 0.08;
			const totalWidth = charWidth * chars.length;
			let currentX = (size - totalWidth) / 2;

			chars.forEach((char, i) => {
				const normX = (i - mid) / (mid || 1);
				const y = 50 * (normX * normX);
				svg += `<text x='${currentX + charWidth / 2}' y='${y + size / 2}' text-anchor='middle' dominant-baseline='middle' style='${style}'>${this.escapeXml(char)}</text>`;
				currentX += charWidth;
			});

			return svg;
		},

		renderValleyShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const curveScale = shapeData.curveScale || (50 * scale);
			const charWidth = size * 0.06;
			const spacing = size * 0.001;
			const totalWidth = chars.length * charWidth + spacing * Math.max(chars.length - 1, 0);
			let currentX = (size - totalWidth) / 2;

			chars.forEach((char, i) => {
				const normX = (i - mid) / (mid || 1);
				const y = -curveScale * (normX * normX) + (curveScale * 0.5);
				const tiltAngle = normX * -40;
				const posX = currentX + charWidth / 2;
				const posY = size / 2 + y;
				svg += `<text x='${posX}' y='${posY}' text-anchor='middle' dominant-baseline='middle' style='${style}' transform='rotate(${tiltAngle} ${posX} ${posY})'>${this.escapeXml(char)}</text>`;
				currentX += charWidth + spacing;
			});

			return svg;
		},

		renderBulgeShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const bulgeAmount = shapeData.bulgeAmount || 1;
			const scales = chars.map((_, i) => {
				const dist = Math.abs(i - mid);
				const maxDist = mid || 1;
				return 1 + bulgeAmount * (1 - dist / maxDist);
			});

			const charWidth = size * 0.05;
			const totalWidth = scales.reduce((acc, s) => acc + charWidth * s, 0);
			let currentX = (size - totalWidth) / 2;

			chars.forEach((char, i) => {
				const charScale = scales[i];
				const scaledWidth = charWidth * charScale;
				const scaledFontSize = shape.fontSize * scale * charScale;
				const scaledStyle = style.replace(
					`font-size:${shape.fontSize * scale}px`,
					`font-size:${scaledFontSize}px`
				);
				svg += `<text x='${currentX + scaledWidth / 2}' y='${size / 2}' text-anchor='middle' dominant-baseline='middle' style='${scaledStyle}'>${this.escapeXml(char)}</text>`;
				currentX += scaledWidth;
			});

			return svg;
		},

		renderFlagShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const waveAmount = shapeData.waveAmount || (20 * scale);
			const charWidth = size * 0.025;
			const spacing = size * 0.002;
			const totalWidth = chars.length * charWidth + spacing * Math.max(chars.length - 1, 0);
			let currentX = (size - totalWidth) / 2;

			chars.forEach((char, i) => {
				// Create a smoother, less intense wave using a lower frequency and amplitude
				const frequency = 0.25; // Reduced from 0.5 for smoother wave
				const y = Math.sin(i * frequency) * (waveAmount * 0.6); // Reduced amplitude to 60%
				const posX = currentX + charWidth / 2;
				const posY = size / 2 + y;
				svg += `<text x='${posX}' y='${posY}' text-anchor='middle' dominant-baseline='middle' style='${style}'>${this.escapeXml(char)}</text>`;
				currentX += charWidth + spacing;
			});

			return svg;
		},

		renderDistortShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const distortAmount = shapeData.distortAmount || 0.5;
			const len = chars.length;
			const scales = chars.map((_, i) => 0.5 + distortAmount * (i / (len - 1 || 1)));
			const charWidth = size * 0.05;
			const totalWidth = scales.reduce((acc, s) => acc + charWidth * s, 0);
			let currentX = (size - totalWidth) / 2;

			chars.forEach((char, i) => {
				const charScale = scales[i];
				const scaledWidth = charWidth * charScale;
				const scaledFontSize = shape.fontSize * scale * charScale;
				const scaledStyle = style.replace(
					`font-size:${shape.fontSize * scale}px`,
					`font-size:${scaledFontSize}px`
				);
				svg += `<text x='${currentX + scaledWidth / 2}' y='${size / 2}' text-anchor='middle' dominant-baseline='middle' style='${scaledStyle}'>${this.escapeXml(char)}</text>`;
				currentX += scaledWidth;
			});

			return svg;
		},

		renderCircleShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const radius = shapeData.radius || ((size / 120) * 0.5);
			const centerX = size / 2;
			const centerY = size / 2;
			const len = chars.length;
			const angleStep = (2 * Math.PI) / len;

			chars.forEach((char, i) => {
				const angle = i * angleStep - Math.PI / 2;
				const x = centerX + Math.cos(angle) * radius;
				const y = centerY + Math.sin(angle) * radius;
				const rotation = (angle * 180 / Math.PI) + 90;
				svg += `<text x='${x}' y='${y}' text-anchor='middle' dominant-baseline='middle' style='${style}' transform='rotate(${rotation} ${x} ${y})'>${this.escapeXml(char)}</text>`;
			});

			return svg;
		},

		renderPinchShape(chars, mid, shapeData, size, style, shape, scale) {
			let svg = "";
			const pinchAmount = shapeData.pinchAmount || 0.5;
			const scales = chars.map((_, i) => {
				const dist = Math.abs(i - mid);
				const maxDist = mid || 1;
				return 1 - pinchAmount * (1 - dist / maxDist);
			});

			const charWidth = size * 0.05;
			const totalWidth = scales.reduce((acc, s) => acc + charWidth * s, 0);
			let currentX = (size - totalWidth) / 2;

			chars.forEach((char, i) => {
				const charScale = scales[i];
				const scaledWidth = charWidth * charScale;
				const scaledFontSize = shape.fontSize * scale * charScale;
				const scaledStyle = style.replace(
					`font-size:${shape.fontSize * scale}px`,
					`font-size:${scaledFontSize}px`
				);
				svg += `<text x='${currentX + scaledWidth / 2}' y='${size / 2}' text-anchor='middle' dominant-baseline='middle' style='${scaledStyle}'>${this.escapeXml(char)}</text>`;
				currentX += scaledWidth;
			});

			return svg;
		},

		buildStyleWithOutline(baseStyle, shape, scale = 1) {
			// This method only adds font weight and style
			let style = baseStyle;
			if (shape.fontWeight) style += ` font-weight:${shape.fontWeight};`;
			if (shape.fontStyle) style += ` font-style:${shape.fontStyle};`;
			return style;
		},

		/**
		 * Create outline SVG using stroke for better clarity
		 * Renders as a separate group underneath text
		 * @param {string} text - Text to outline
		 * @param {object} shape - Shape configuration
		 * @param {number} size - Canvas/material size
		 * @param {number} scale - Scale factor (1 for preview, size/120 for material)
		 * @returns {string} SVG outline group
		 */
		createOutlineGroup(text, shape, size, scale = 1) {
			if (!shape.outlineEnabled || !shape.outlineColor || shape.outlineThickness <= 0) {
				return "";
			}

			const scaledFontSize = shape.fontSize * scale;
			// Use proper stroke settings for clear outline without blur
			const outlineStyle = `font-size:${scaledFontSize}px; fill:none; stroke:${shape.outlineColor}; stroke-width:${shape.outlineThickness * scale}px; font-family:${shape.fontFamily}; font-weight:${shape.fontWeight || 'normal'}; font-style:${shape.fontStyle || 'normal'}; stroke-linejoin:round; stroke-linecap:round; paint-order:stroke fill; text-rendering:geometricPrecision;`;

			let svg = `<g id='outline-group'>`;

			if (shape.textShape && shape.textShape !== "none") {
				svg += this.renderShapeText(text, shape, size, outlineStyle, shape.textShape, scale);
			} else {
				const cx = size / 2;
				const cy = size / 2;
				svg += `<text x='${cx}' y='${cy}' text-anchor='middle' dominant-baseline='middle' style='${outlineStyle}'>${this.escapeXml(text)}</text>`;
			}

			svg += `</g>`;
			return svg;
		},

		escapeXml(str) {
			return String(str).replace(/[<>&'"]/g, function (c) {
				switch (c) {
					case '<': return '&lt;';
					case '>': return '&gt;';
					case '&': return '&amp;';
					case '\'': return '&apos;';
					case '"': return '&quot;';
				}
			});
		},

		getTextPathForShape(shapeType, intensity, size = 120, text = "") {
			const intensityFactor = (intensity || 50) / 100;
			const sizeRatio = size / 120;
			const len = text.length || 1;

			switch (shapeType) {
				case "arch": {
					// Calculate radius based on intensity with gradual progression
					// Smaller base radius for more subtle effect at lower intensities
					// At 50%+ intensity, it should form a proper half-circle arch
					const baseRadius = size === 120 ? 30 : 30 * sizeRatio;
					// Use quadratic scaling for smoother transition (intensityFactor^1.5)
					const scaledIntensity = Math.pow(intensityFactor, 1.2);
					const radius = baseRadius * Math.max(0.15, scaledIntensity);
					const angleRange = Math.PI * 0.8;
					const startAngle = -Math.PI / 2 - angleRange / 2;
					return { type: "arch", radius, len, angleRange, startAngle, intensityFactor };
				}
				case "valley": {
					const mid = (len - 1) / 2;
					const curveScale = (size === 120 ? 50 : 5 * sizeRatio) * intensityFactor;
					return { type: "valley", mid, len, curveScale };
				}
				case "bulge": {
					const mid = (len - 1) / 2;
					const bulgeAmount = (size === 120 ? 0.8 : 0.5) * intensityFactor;
					return { type: "bulge", mid, len, bulgeAmount };
				}
				case "flag":
				case "wave": {
					const waveAmount = 20 * (size === 120 ? 1 : sizeRatio) * intensityFactor;
					return { type: "flag", len, waveAmount };
				}
				case "distort": {
					const distortAmount = 0.5 * intensityFactor;
					return { type: "distort", len, distortAmount };
				}
				case "circle": {
					const radius = (size === 120 ? 110 : 11 * sizeRatio) * intensityFactor;
					return { type: "circle", radius, len };
				}
				case "pinch": {
					const mid = (len - 1) / 2;
					const pinchAmount = intensityFactor;
					return { type: "pinch", mid, len, pinchAmount };
				}
				default:
					return { type: "none" };
			}
		},

		updateLastTextShape(key, value) {
			if (!this.svgShapes.length) return;
			const last = this.svgShapes[this.svgShapes.length - 1];
			if (last.type === "text") {
				last[key] = value;
				this.updateSvgPreview();
				this.addSvgLayer();
			}
		},

		addSvgLayer() {
			const panel = this.currentPanel();
			if (!panel?.texture()) {
				console.error("Cannot add SVG layer: panel or texture not found");
				return;
			}

			const size = panel.settings.build.parameters.size;

			if (!this.currentSvgLayer) {
				this.currentSvgLayer = new WrapviewSvgLayer(WrapviewUtils.guid(), {
					size: { width: size, height: size },
					pivot: { x: 0.5, y: 0.5 },
					position: { x: size / 2, y: size / 2 },
					angle: 0,
				});
			}

			panel.texture().beginEditing().then(() => {
				const svgData = this.buildSvgDataForMaterial(size);
				const layers = panel.texture().layers();
				let layerIndex = panel.texture().addLayer(this.currentSvgLayer);

				for (let i = 0; i < layers.length; i++) {
					if (layers[i].id === this.currentSvgLayer.id) {
						layerIndex = i;
						break;
					}
				}

				this.currentSvgLayer.load({ svgData }).then(() => {
					const mat = this.materials.get("FRONT_BODY");
					if (mat) {
						mat.texture().editLayer(layerIndex);
						mat.texture().render();
					}
				}).catch((err) => console.error("Error loading SVG layer:", err));
			}).catch((err) => console.error("Error in beginEditing():", err));
		},

		/**
		 * Build SVG data scaled to material size
		 * Uses renderShapeText for unified rendering
		 */
		buildSvgDataForMaterial(size) {
			const scale = size / 120;
			// Add SVG filter for sharp text rendering without blur
			var svg = `<svg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' width='${size}' height='${size}'>`;
			svg += `<defs><filter id='textFilter' x='-50%' y='-50%' width='200%' height='200%'><feGaussianBlur in='SourceGraphic' stdDeviation='0' /></filter></defs>`;

			for (var shape of this.svgShapes) {
				if (shape.type === "text") {
					// Render outline first (underneath) as separate layer
					svg += this.createOutlineGroup(shape.value, shape, size, scale);

					// Render text on top with anti-aliasing settings
					const scaledFontSize = shape.fontSize * scale;
					const baseStyle = `font-size:${scaledFontSize}px; fill:${shape.fill}; font-family:${shape.fontFamily}; font-weight:${shape.fontWeight || 'normal'}; font-style:${shape.fontStyle || 'normal'}; -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale; text-rendering:geometricPrecision; filter:url(#textFilter);`;

					if (shape.textShape && shape.textShape !== "none") {
						svg += this.renderShapeText(shape.value, shape, size, baseStyle, shape.textShape, scale);
					} else {
						const cx = size / 2;
						const cy = size / 2;
						svg += `<text x='${cx}' y='${cy}' text-anchor='middle' dominant-baseline='middle' style='${baseStyle}'>${this.escapeXml(shape.value)}</text>`;
					}
				}
			}

			svg += `</svg>`;
			return svg;
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