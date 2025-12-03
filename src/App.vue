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
				<div style="padding: 10px; display: flex; flex-direction: row; flex-wrap: wrap; gap: 15px;">
					<label style="flex: 1; min-width: 200px;">
						<span style="display: block; margin-bottom: 5px; font-weight: bold">Edit Text</span>
						<input type="text" v-model="svgTextValue" placeholder="Enter text" style="width: 100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px;" />
					</label>
					<label style="flex: 0 0 150px; min-width: 150px;">
						<span style="display: block; margin-bottom: 5px; font-weight: bold">Text Size (px)</span>
						<input type="number" v-model.number="svgFontSize" min="1" max="120" step="1" style="width: 100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px;" />
					</label>
				</div>
			</div>
			<div id="tab2" :class="{ 'display-none': activeTab !== 1 }">
				<div class="color-container">
					<div class="color" style="background: linear-gradient(135deg, #ffffff 47%, #ff0000 47%, #ff0000 53%, #ffffff 53%); border: 1px solid #ccc" @click="svgTextColor = 'transparent'"
						title="Transparent"></div>
					<div class="color" style="background-color: #000000" @click="svgTextColor = '#000000'"
						title="Black"></div>
					<div class="color" style="background-color: #ffffff; border: 1px solid #ccc"
						@click="svgTextColor = '#FFFFFF'" title="White"></div>
					<div class="color" style="background-color: #808080" @click="svgTextColor = '#808080'" title="Gray">
					</div>
					<div class="color" style="background-color: #c0c0c0" @click="svgTextColor = '#C0C0C0'"
						title="Silver"></div>
					<div class="color" style="background-color: #ff0000" @click="svgTextColor = '#FF0000'" title="Red">
					</div>
					<div class="color" style="background-color: #8b0000" @click="svgTextColor = '#8B0000'"
						title="Dark Red"></div>
					<div class="color" style="background-color: #ffa500" @click="svgTextColor = '#FFA500'"
						title="Orange"></div>
					<div class="color" style="background-color: #ff8c00" @click="svgTextColor = '#FF8C00'"
						title="Dark Orange">
					</div>
					<div class="color" style="background-color: #ffff00" @click="svgTextColor = '#FFFF00'"
						title="Yellow"></div>
					<div class="color" style="background-color: #ffd700" @click="svgTextColor = '#FFD700'" title="Gold">
					</div>
					<div class="color" style="background-color: #00ff00" @click="svgTextColor = '#00FF00'" title="Lime">
					</div>
					<div class="color" style="background-color: #008000" @click="svgTextColor = '#008000'"
						title="Green"></div>
					<div class="color" style="background-color: #006400" @click="svgTextColor = '#006400'"
						title="Dark Green">
					</div>
					<div class="color" style="background-color: #00ffff" @click="svgTextColor = '#00FFFF'" title="Cyan">
					</div>
					<div class="color" style="background-color: #008b8b" @click="svgTextColor = '#008B8B'"
						title="Dark Cyan">
					</div>
					<div class="color" style="background-color: #0000ff" @click="svgTextColor = '#0000FF'" title="Blue">
					</div>
					<div class="color" style="background-color: #000080" @click="svgTextColor = '#000080'" title="Navy">
					</div>
					<div class="color" style="background-color: #4169e1" @click="svgTextColor = '#4169E1'"
						title="Royal Blue">
					</div>
					<div class="color" style="background-color: #800080" @click="svgTextColor = '#800080'"
						title="Purple"></div>
					<div class="color" style="background-color: #4b0082" @click="svgTextColor = '#4B0082'"
						title="Indigo"></div>
					<div class="color" style="background-color: #ff00ff" @click="svgTextColor = '#FF00FF'"
						title="Magenta"></div>
					<div class="color" style="background-color: #ff1493" @click="svgTextColor = '#FF1493'"
						title="Deep Pink">
					</div>
					<div class="color" style="background-color: #ffc0cb" @click="svgTextColor = '#FFC0CB'" title="Pink">
					</div>
					<div class="color" style="background-color: #a52a2a" @click="svgTextColor = '#A52A2A'"
						title="Brown"></div>
					<div class="color" style="background-color: #d2691e" @click="svgTextColor = '#D2691E'"
						title="Chocolate">
					</div>
					<div class="color" style="background-color: #f5f5dc" @click="svgTextColor = '#F5F5DC'"
						title="Beige"></div>
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
						<p>Enable Outline</p>
						<input type="checkbox" v-model="svgOutlineEnabled"
							style="width: 20px; height: 20px; cursor: pointer;" />
					</div>
					<p>{{ svgOutlineThickness }}pt</p>
				</div>
				<div class="color-container">
					<div class="color" style="background-color: #000000" @click="svgOutlineColor = '#000000'"
						title="Black"></div>
					<div class="color" style="background-color: #ffffff; border: 1px solid #ccc"
						@click="svgOutlineColor = '#FFFFFF'" title="White"></div>
					<div class="color" style="background-color: #ff0000" @click="svgOutlineColor = '#FF0000'"
						title="Red"></div>
					<div class="color" style="background-color: #00ff00" @click="svgOutlineColor = '#00FF00'"
						title="Lime"></div>
					<div class="color" style="background-color: #0000ff" @click="svgOutlineColor = '#0000FF'"
						title="Blue"></div>
					<div class="color" style="background-color: #ffff00" @click="svgOutlineColor = '#FFFF00'"
						title="Yellow"></div>
					<div class="color" style="background-color: #ff00ff" @click="svgOutlineColor = '#FF00FF'"
						title="Magenta"></div>
				</div>
				<div class="outline-footer" style="display: flex; flex-direction: row; align-items: center; gap: 10px;">
					<p style="margin: 0;">Outline Thickness (pt)</p>
					<input type="number" v-model.number="svgOutlineThickness" min="0" max="10" step="0.01" style="width: 80px; padding: 8px; border: 1px solid #ccc; border-radius: 4px;" />
				</div>
			</div>
			<div id="tab5" :class="{ 'display-none': activeTab !== 4 }">
				<div class="style-content">
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'none' }"
						@click="svgTextShape = 'none'">
						<h2>None</h2>
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'arch' }"
						@click="svgTextShape = 'arch'">
						<img src="/icons/arch.png" />
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'bridge' }"
						@click="svgTextShape = 'bridge'">
						<img src="/icons/bridge.png" />
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'valley' }"
						@click="svgTextShape = 'valley'">
						<img src="/icons/bridge.png" style="transform: rotate(180deg);" />
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'bulge' }"
						@click="svgTextShape = 'bulge'">
						<img src="/icons/buldge.png" />
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'flag' }"
						@click="svgTextShape = 'flag'">
						<img src="/icons/flag.png" />
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'distort' }"
						@click="svgTextShape = 'distort'">
						<img src="/icons/wave.png" />
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'angle' }"
						@click="svgTextShape = 'angle'">
						<img src="/icons/angle.png" />
					</div>
					<div class="fontStyle" :class="{ 'active-shape': svgTextShape === 'circle' }"
						@click="svgTextShape = 'circle'">
						<img src="/icons/circle.png" />
					</div>
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
	WrapviewSVGEditor,
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
			svgTextColor: "#000000",
			svgFontSize: 24,
			svgTextDecoration: "",
			svgFontFamily: "Arial",
			svgTextValue: "WARRIORS",
			svgTextShape: "none",
			svgOutlineColor: "#000000",
			svgOutlineThickness: 0.5,
			svgOutlineEnabled: false,
			svgInitialized: false,
			currentSvgLayer: null,
			svgEditor: null,
		};
	},
	mounted() {
		this.$nextTick(() => {
			this.initSVGEditor();
		});
	},
	watch: {
		svgTextColor(val) {
			this.renderFabricText();
		},
		svgFontSize(val) {
			this.renderFabricText();
		},
		svgTextDecoration(val) {
			this.renderFabricText();
		},
		svgFontFamily(val) {
			this.renderFabricText();
		},
		svgTextValue(val) {
			this.renderFabricText();
		},
		svgTextShape(val) {
			this.renderFabricText();
		},
		svgOutlineColor(val) {
			this.renderFabricText();
		},
		svgOutlineThickness(val) {
			this.renderFabricText();
		},
		svgOutlineEnabled(val) {
			this.renderFabricText();
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
					intensity: 0.5,
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

		initSVGEditor() {
			if (this.svgInitialized) return;
			
			// Create a hidden container for the SVG editor
			const container = document.createElement('div');
			container.id = 'hidden-svg-editor';
			container.style.display = 'none';
			document.body.appendChild(container);
			
			// Initialize WrapviewSVGEditor
			this.svgEditor = new WrapviewSVGEditor(this.$refs["wrapView"].instance());
			
			// Set up change callback to update 3D model
			this.svgEditor.setOnChange((dataUrl) => {
				this.addSvgLayer();
			});
			
			// Attach editor to hidden container
			this.svgEditor.attachTo(container);
			
			this.svgInitialized = true;
		},

		renderFabricText() {
			if (!this.svgEditor || !this.svgEditor._canvas) return;
			
			// Update the editor's internal configuration directly
			const canvas = this.svgEditor._canvas;
			const config = this.svgEditor._index3Config;
			
			// Update font size
			config.fontSize = this.svgFontSize;
			
			// Get DOM elements that the editor uses
			const textInput = document.getElementById('textInput');
			const fontFamily = document.getElementById('fontFamily');
			const textColor = document.getElementById('textColor');
			const outlineEnabled = document.getElementById('outlineEnabled');
			const outlineColor = document.getElementById('outlineColor');
			const outlineWidth = document.getElementById('outlineWidth');
			
			if (textInput) textInput.value = this.svgTextValue;
			if (fontFamily) fontFamily.value = this.svgFontFamily;
			if (textColor) textColor.value = this.svgTextColor;
			if (outlineEnabled) outlineEnabled.checked = this.svgOutlineEnabled;
			if (outlineColor) outlineColor.value = this.svgOutlineColor;
			if (outlineWidth) outlineWidth.value = this.svgOutlineThickness;
			
			// Set the current effect
			this.svgEditor._currentEffect = this.svgTextShape || 'none';
			
			// Trigger a re-render using the editor's internal render logic
			canvas.clear();
			
			const getBaseTextOptions = () => {
				let fontWeight = 'normal';
				let fontStyle = 'normal';
				if (this.svgTextDecoration === 'bold') fontWeight = 'bold';
				if (this.svgTextDecoration === 'italic') fontStyle = 'italic';
				if (this.svgTextDecoration === 'bold italic') {
					fontWeight = 'bold';
					fontStyle = 'italic';
				}
				
				return {
					fontSize: this.svgFontSize,
					fontFamily: this.svgFontFamily,
					fontWeight: fontWeight,
					fontStyle: fontStyle,
					fill: this.svgTextColor,
					originX: 'center',
					originY: 'center',
					stroke: this.svgOutlineEnabled ? this.svgOutlineColor : undefined,
					strokeWidth: this.svgOutlineEnabled ? this.svgOutlineThickness : 0
				};
			};
			
			// Use the effects from the WrapviewSVGEditor (directly access the effects)
			const text = this.svgTextValue;
			const effect = this.svgTextShape || 'none';
			const options = getBaseTextOptions();
			const radius = 150;
			
			let obj;
			
			switch (effect) {
				case 'none':
					obj = new fabric.Text(text, {
						...options,
						left: canvas.width / 2,
						top: canvas.height / 2
					});
					break;
				case 'arch':
					obj = this._createArchEffect(text, options, radius, canvas);
					break;
				case 'bridge':
					obj = this._createBridgeEffect(text, options, canvas);
					break;
				case 'valley':
					obj = this._createValleyEffect(text, options, canvas);
					break;
				case 'bulge':
					obj = this._createBulgeEffect(text, options, canvas);
					break;
				case 'flag':
					obj = this._createFlagEffect(text, options, canvas);
					break;
				case 'distort':
					obj = this._createDistortEffect(text, options, canvas);
					break;
				case 'circle':
					obj = this._createCircleEffect(text, options, canvas);
					break;
				default:
					obj = new fabric.Text(text, {
						...options,
						left: canvas.width / 2,
						top: canvas.height / 2
					});
			}
			
			if (obj) {
				canvas.add(obj);
				canvas.centerObject(obj);
				obj.setCoords();
				canvas.requestRenderAll();
				
				// Update the 3D model
				this.addSvgLayer();
			}
		},
		
		// Helper methods that mirror WrapviewSVGEditor effects
		_createArchEffect(text, options, radius, canvas) {
			const group = new fabric.Group([], {
				left: canvas.width / 2,
				top: canvas.height / 2,
				originX: 'center',
				originY: 'center'
			});
			const len = text.length;
			for (let i = 0; i < len; i++) {
				const char = text[i];
				const charAngle = -Math.PI / 2 + (i - (len - 1) / 2) * 0.2;
				const c = new fabric.Text(char, {
					...options,
					left: Math.cos(charAngle) * radius,
					top: Math.sin(charAngle) * radius,
					angle: (charAngle * 180 / Math.PI) + 90
				});
				group.addWithUpdate(c);
			}
			return group;
		},
		
		_createBridgeEffect(text, options, canvas) {
			const group = new fabric.Group([], {
				left: canvas.width / 2,
				top: canvas.height / 2,
				originX: 'center',
				originY: 'center'
			});
			const chars = text.split('').map(c => new fabric.Text(c, options));
			const totalWidth = chars.reduce((acc, c) => acc + c.width, 0);
			let currentX = -totalWidth / 2;
			const len = text.length;
			const mid = (len - 1) / 2;
			chars.forEach((ch, i) => {
				const normX = (i - mid) / (mid || 1);
				const y = 50 * (normX * normX);
				ch.set({ left: currentX + ch.width / 2, top: y, originX: 'center', originY: 'center' });
				currentX += ch.width;
				group.addWithUpdate(ch);
			});
			return group;
		},
		
		_createValleyEffect(text, options, canvas) {
			const group = new fabric.Group([], {
				left: canvas.width / 2,
				top: canvas.height / 2,
				originX: 'center',
				originY: 'center'
			});
			const chars = text.split('').map(c => new fabric.Text(c, options));
			const totalWidth = chars.reduce((acc, c) => acc + c.width, 0);
			let currentX = -totalWidth / 2;
			const mid = (chars.length - 1) / 2;
			chars.forEach((ch, i) => {
				const normX = (i - mid) / (mid || 1);
				const y = -50 * (normX * normX) + 25;
				ch.set({ left: currentX + ch.width / 2, top: y, originX: 'center', originY: 'center' });
				currentX += ch.width;
				group.addWithUpdate(ch);
			});
			return group;
		},
		
		_createBulgeEffect(text, options, canvas) {
			const group = new fabric.Group([], {
				left: canvas.width / 2,
				top: canvas.height / 2,
				originX: 'center',
				originY: 'center'
			});
			const chars = text.split('').map(c => new fabric.Text(c, options));
			const mid = (chars.length - 1) / 2;
			chars.forEach((ch, i) => {
				const dist = Math.abs(i - mid);
				const maxDist = mid || 1;
				const scale = 1 + 0.8 * (1 - dist / maxDist);
				ch.set({ fontSize: options.fontSize * scale });
			});
			let currentX = -chars.reduce((acc, c) => acc + c.getScaledWidth(), 0) / 2;
			chars.forEach(ch => {
				ch.set({ left: currentX + ch.getScaledWidth() / 2, top: 0, originX: 'center', originY: 'center' });
				currentX += ch.getScaledWidth();
				group.addWithUpdate(ch);
			});
			return group;
		},
		
		_createFlagEffect(text, options, canvas) {
			const group = new fabric.Group([], {
				left: canvas.width / 2,
				top: canvas.height / 2,
				originX: 'center',
				originY: 'center'
			});
			const chars = text.split('').map(c => new fabric.Text(c, options));
			let currentX = -chars.reduce((acc, c) => acc + c.width, 0) / 2;
			chars.forEach((ch, i) => {
				const y = Math.sin(i * 0.5) * 20;
				ch.set({ left: currentX + ch.width / 2, top: y, originX: 'center', originY: 'center' });
				currentX += ch.width;
				group.addWithUpdate(ch);
			});
			return group;
		},
		
		_createDistortEffect(text, options, canvas) {
			const group = new fabric.Group([], {
				left: canvas.width / 2,
				top: canvas.height / 2,
				originX: 'center',
				originY: 'center'
			});
			const chars = text.split('').map(c => new fabric.Text(c, options));
			let currentX = -chars.reduce((acc, c) => acc + c.width, 0) / 2;
			chars.forEach((ch, i) => {
				const skew = (i % 2 === 0) ? -20 : 20;
				ch.set({ left: currentX + ch.width / 2, top: 0, skewY: skew, originX: 'center', originY: 'center' });
				currentX += ch.width;
				group.addWithUpdate(ch);
			});
			return group;
		},
		
		_createCircleEffect(text, options, canvas) {
			const group = new fabric.Group([], {
				left: canvas.width / 2,
				top: canvas.height / 2,
				originX: 'center',
				originY: 'center'
			});
			const radius = 120;
			const len = text.length;
			const angleStep = (2 * Math.PI) / len;
			for (let i = 0; i < len; i++) {
				const char = text[i];
				const angle = i * angleStep - Math.PI / 2;
				const ch = new fabric.Text(char, {
					...options,
					left: Math.cos(angle) * radius,
					top: Math.sin(angle) * radius,
					angle: (angle * 180 / Math.PI) + 90,
					originX: 'center',
					originY: 'center'
				});
				group.addWithUpdate(ch);
			}
			return group;
		},

		addSvgLayer() {
			if (!this.svgEditor || !this.svgEditor._canvas) return;
			
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

			var color = new WrapviewParameter(panel, "textColor");
			color.set({
				type: "fixed",
				value: "#1c5982",
				descriptor: "Black",
			});
			panel.settings.buildable.diffuse.baseLayer().setColorParameter(color);
            panel.settings.buildable.diffuse.baseLayer().setNeedsUpdate();

			// Begin editing the texture before adding layers
			panel
				.texture()
				.beginEditing()
				.then(() => {
					// Get data URL from SVG editor's canvas
					const svgData = this.svgEditor.getDataURL();

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
</style>
