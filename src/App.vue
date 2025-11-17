<template>
	<Wrapview ref="wrapView" @onInitalized="environmentMounted"></Wrapview>
	<div id="orbitControls"></div>
    <section class="bottom-panel">
		<div class="thumb-container">
			<div class="thumb"></div>
		</div>
		<div class="top-tabs">
			<div class="tab">
				<div class="image"></div>
				<p>Colors</p>
			</div>
			<div class="tab">
				<div class="image"></div>
				<p>Layers</p>
			</div>
			<div class="tab active">
				<div class="image"></div>
				<p>Text Editor</p>
			</div>
			<div class="tab">
				<div class="image"></div>
				<p>Images</p>
			</div>
			<div class="tab">
				<div class="image"></div>
				<p>Personalize</p>
			</div>
			<div class="tab">
				<div class="image"></div>
				<p>Done</p>
			</div>
		</div>
		<div class="bottom-tabs">
			<div class="tab" :class="{ 'active' : activeTab === 0 }" v-on:click="changeTab(0)">
				<div class="image"></div>
				<p>Edit Text</p>
			</div>
			<div class="tab" :class="{ 'active' : activeTab === 1 }" v-on:click="changeTab(1)">
				<div class="image"></div>
				<p>Edit Color</p>
			</div>
			<div class="tab" :class="{ 'active' : activeTab === 2 }" v-on:click="changeTab(2)">
				<div class="image"></div>
				<p>Font</p>
			</div>
			<div class="tab" :class="{ 'active' : activeTab === 3 }" v-on:click="changeTab(3)">
				<div class="image"></div>
				<p>Outline</p>
			</div>
			<div class="tab" :class="{ 'active' : activeTab === 4 }" v-on:click="changeTab(4)">
				<div class="image"></div>
				<p>Shape</p>
			</div>
		</div>
		<div class="content">
			<div id="tab1" :class="{'display-none': activeTab !== 0}">
				<WrapviewInputControl label="Edit Text" />
				<WrapviewInputControl label="Text Size" type="range" />
			</div>
			<div id="tab2" :class="{'display-none': activeTab !== 1}">
				<div class="color-container">
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
					<div class="color"></div>
				</div>
			</div>
			<div id="tab3" :class="{'display-none': activeTab !== 2}">
				<div class="font-variants">
					<h4>Warriors</h4>
					<p>Asap Condensed</p>
				</div>
				<div class="font-variants">
					<h4>Warriors</h4>
					<p>Baloo</p>
				</div>
				<div class="font-variants">
					<h4>Warriors</h4>
					<p>Caprasimo</p>
				</div>
				<div class="font-variants">
					<h4>Warriors</h4>
					<p>Caramel</p>
				</div>
			</div>
			<div id="tab4" :class="{'display-none': activeTab !== 3}">
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
						<input  type="range" />
						<p>Very Thick</p>
					</div>
				</div>
			</div>
			<div id="tab5" :class="{'display-none': activeTab !== 4}">
				<div class="style-content">
					<div class="fontStyle">
						<h2>Arch</h2>
					</div>
					<div class="fontStyle">
						<h2>Bridge</h2>
					</div>
					<div class="fontStyle">
						<h2>Buldge</h2>
					</div>
					<div class="fontStyle">
						<h2>Flag</h2>
					</div>
					<div class="fontStyle">
						<h2>Circle</h2>
					</div>
					<div class="fontStyle">
						<h2>Distort</h2>
					</div>
					<div class="fontStyle">
						<h2>Valley</h2>
					</div>
					<div class="fontStyle">
						<h2>Pinch</h2>
					</div>
				</div>
				<div class="style-footer">
					<p>Shape Intensity</p>
					<input type="range" />
				</div>
			</div>
		</div>
	</section>
</template>
<script>
import { Wrapview, Wizard, WrapviewInputControl } from '@etlok-systems/wrapview-vue'
import {
	OrbitControls,
	WrapviewSettings,
	WrapviewObject,
	WrapviewMaterialSet,
	WrapviewShadowMaterial,
	WrapviewTexturedMaterial,
	WrapviewStitchMaterial,
	WrapviewLight
} from "@etlok-systems/wrapview"

export default {
	components: {Wrapview, WrapviewInputControl},
	data() {
		return {
			size: {
				height: 0,
				width: 0,
			},
			activeTab: 0
		}
	},
	methods: {
		changeTab(tab) {
			this.activeTab = tab
		},
		environmentMounted() {
			this.calculateDimensions()
			this.loadEnvironment().then(() => this.loadLights().then(() => this.loadMaterials().then(({ materials }) => this.loadObjects(materials).then(() => {
			    this.$refs['wrapView'].show()
			    this.$refs['wrapView'].instance().animate()
			}))))
		},
		calculateDimensions() {
			this.size = {
				width: window.innerWidth,
				height: window.innerHeight
			}
		},
		loadEnvironment() {
			return new Promise((resolve, reject) => {
				WrapviewSettings.init();
				var bgColor = 0xf3f4f6;

				this.$refs['wrapView'].viewer().init({
					renderer: {
						antialias: true,
						alpha: false,
						preserveDrawingBuffer: true,
						background: bgColor
					},
					mode: this.mode,
					agent: this.size
				});

				var orbitController = new OrbitControls(this.$refs['wrapView'].instance().camera(), document.getElementById('orbitControls'));
				orbitController.enablePan = false;
				orbitController.enableZoom = true;
				orbitController.enableDamping = true;
				orbitController.minDistance = 1;
				orbitController.maxDistance = 2;

				//Set Initial Camera
				this.resetCamera();

				this.$refs['wrapView'].instance().setController(orbitController);
				resolve();
			});
		},
		resetCamera() {
			let x = 0;
			let y = 0;
			let z = -1.5;
			this.$refs['wrapView'].instance().camera().position.set(x, y, z);
		},
		loadLights() {
			return new Promise((resolve, reject) => {

				const dirLight = new WrapviewLight({
					type: 'directional',
					color: 0xffffff,
					intensity: 1,
					position: { x: 1, y: 1, z: -2 },
					target: { x: 0, y: 0, z: 0 }
				});

				const hemLight = new WrapviewLight({
					type: 'hemisphere',
					color: 0xffffff,
					intensity: 1
				});

				this.$refs['wrapView'].instance().scene().add(hemLight.createLight(), dirLight.createLight());

				const envPaths = [
					'environment/px.jpg',
					'environment/nx.jpg',
					'environment/py.jpg',
					'environment/ny.jpg',
					'environment/pz.jpg',
					'environment/nz.jpg',
				];

				const envLight = new WrapviewLight({ type: 'ambient', intensity: 1 });
				this.$refs['wrapView'].instance().scene().add(envLight.createLight());

				envLight.loadEnvironmentMap(envPaths)
					.then(texture => {
						this.$refs['wrapView'].instance().scene().environment = texture;
					})
					.catch(err => console.error('Failed to load environment map:', err));


				resolve();
			});
		},
		loadMaterials() {
			return new Promise((resolve, reject) => {
				this.$refs['wrapView'].instance().updateOffsets();
				const promises = [];

				var materials = new WrapviewMaterialSet();
				const shadow = new WrapviewShadowMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						alpha: 'https://combibmark.s3.amazonaws.com/models/shadow_ultra_light_inverted.png'
					}
				});

				const collar = new WrapviewTexturedMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						diffuse: './3001C_SMALL/textures/F_3001C_SMALL_diffuse_1005.png',
						normal: './3001C_SMALL/textures/F_3001C_SMALL_normal_1005.png',
						alpha: './3001C_SMALL/textures/F_3001C_SMALL_opacity_1005.png',
						roughness: './3001C_SMALL/textures/F_3001C_SMALL_roughness_1005.png',
						metalness: './3001C_SMALL/textures/F_3001C_SMALL_metalness_1005.png',
					}
				});

				const backNeckTape = new WrapviewTexturedMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						diffuse: './3001C_SMALL/textures/F_3001C_SMALL_diffuse_1006.png',
						normal: './3001C_SMALL/textures/F_3001C_SMALL_normal_1006.png',
						alpha: './3001C_SMALL/textures/F_3001C_SMALL_opacity_1006.png',
						roughness: './3001C_SMALL/textures/F_3001C_SMALL_roughness_1006.png',
						metalness: './3001C_SMALL/textures/F_3001C_SMALL_metalness_1006.png',
					}
				});

				const leftArmSleeve = new WrapviewTexturedMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						diffuse: './3001C_SMALL/textures/F_3001C_SMALL_diffuse_1003.png',
						normal: './3001C_SMALL/textures/F_3001C_SMALL_normal_1003.png',
						alpha: './3001C_SMALL/textures/F_3001C_SMALL_opacity_1003.png',
						roughness: './3001C_SMALL/textures/F_3001C_SMALL_roughness_1003.png',
						metalness: './3001C_SMALL/textures/F_3001C_SMALL_metalness_1003.png',
					}
				});

				const rightArmSleeve = new WrapviewTexturedMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						diffuse: './3001C_SMALL/textures/F_3001C_SMALL_diffuse_1004.png',
						normal: './3001C_SMALL/textures/F_3001C_SMALL_normal_1004.png',
						alpha: './3001C_SMALL/textures/F_3001C_SMALL_opacity_1004.png',
						roughness: './3001C_SMALL/textures/F_3001C_SMALL_roughness_1004.png',
						metalness: './3001C_SMALL/textures/F_3001C_SMALL_metalness_1004.png',
					}
				});

				const frontBody = new WrapviewTexturedMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						diffuse: './3001C_SMALL/textures/F_3001C_SMALL_common.png',
						normal: './3001C_SMALL/textures/F_3001C_SMALL_normal_1001.png',
						alpha: './3001C_SMALL/textures/F_3001C_SMALL_opacity_1001.png',
						roughness: './3001C_SMALL/textures/F_3001C_SMALL_roughness_1001.png',
						metalness: './3001C_SMALL/textures/F_3001C_SMALL_metalness_1001.png',
					}
				});

				const backBody = new WrapviewTexturedMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						diffuse: './3001C_SMALL/textures/F_3001C_SMALL_common.png',
						normal: './3001C_SMALL/textures/F_3001C_SMALL_normal_1002.png',
						alpha: './3001C_SMALL/textures/F_3001C_SMALL_opacity_1002.png',
						roughness: './3001C_SMALL/textures/F_3001C_SMALL_roughness_1002.png',
						metalness: './3001C_SMALL/textures/F_3001C_SMALL_metalness_1002.png',
					}
				});

				const stitches = new WrapviewStitchMaterial(this.$refs['wrapView'].instance(), {
					resources: {
						diffuse: './3001C_SMALL/textures/Basic_Offset_2193.png'
					}
				});

				promises.push(
					collar.init(),
					backNeckTape.init(),
					leftArmSleeve.init(),
					rightArmSleeve.init(),
					frontBody.init(),
					backBody.init(),
					shadow.init(),
					stitches.init()
				)

				materials.add('COLLAR', collar);
				materials.add('BACK_NECK_TAPE', backNeckTape);
				materials.add('LEFT_ARM_SLEEVE', leftArmSleeve);
				materials.add('RIGHT_ARM_SLEEVE', rightArmSleeve);
				materials.add('FRONT_BODY', frontBody);
				materials.add('BACK_BODY', backBody);
				materials.add('EXT_Stitches', stitches);
				materials.add('99_ShadowPanel', shadow);

				const allPromises = Promise.all(promises);
				allPromises.then(() => {
					resolve({
						materials: materials
					});
				}, (e) => {
					console.log("Error!", e)
				})
			});
		},
		loadObjects(materials) {
			return new Promise((resolve, reject) => {
				const item = new WrapviewObject({
					transform: {
						rotation: {
							y: -Math.PI
						},
						position: {
							y: 0.2
						},
						scale: {
							x: 0.8, y: 0.8, z: 0.8
						}
					}
				});
				item.setMaterials(materials)
				item.load("./3001C_SMALL/3001C_SMALL_LOD0.glb")
					.then(() => {
						this.$refs['wrapView'].instance().addObject(item)
					});
				resolve();
			});
		}
	}
}
</script>

<style lang="css">
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: sans-serif;
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

.top-tabs, .bottom-tabs {
	display: flex;
	justify-content: space-between;
	align-items: center;
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
	font-size: 12px;
}

.tab.active {
	color: blue;
}

.image {
	height: 28px;
	width: 28px;
	background-color: black;
	border-radius: 50%;
}

.tab.active .image{
	background-color: blue;
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
.outline-footer div{
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

.style-content .fontStyle{
	width: 25%;
	text-align: center;
	padding: 5px;
	margin-bottom: 10px;
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