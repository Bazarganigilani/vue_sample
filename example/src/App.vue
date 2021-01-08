<template>
    <div id="app">
        <h1 class="mb-4 mt-2 alert-success">Vue Image Cropper</h1>
        <div>
<div v-show="showResult1">
                result image 1
            </div>
            <b-img :src="resultImage1" alt="Responsive image" fluid v-show="showResult1"></b-img>
            <div v-show="showResult1">
                <b-link :href="resultImage1" download="result.png">Download Image</b-link>
            </div>
<div v-show="showResult2">
                result image 2
            </div>
            <b-img :src="resultImage2" alt="Responsive image" fluid v-show="showResult2"></b-img>
            <div v-show="showResult2">
                <b-link :href="resultImage2" download="result.png">Download Image</b-link>
            </div>
        </div>
        <div>
<polygonCrop 
    :canvasClass="'some-class'"
    :height="600"
    :imageSource="imgSrc1"
    :showCanvas="show1"
    :showPointer="showPointer"
    :width="800"
    ref="canvas1"
></polygonCrop>
<polygonCrop 
    :canvasClass="'some-class'"
    :height="600"
    :imageSource="imgSrc2"
    :showCanvas="show2"
    :showPointer="showPointer"
    :width="800"
    ref="canvas2"
></polygonCrop>
        </div>
        <b-row>
            <b-col>
                <b-form-group>
                    <b-form-file
                            :state="Boolean(file)"
                            @change="setImage"
                            accept="image/*"
                            class="col-sm-6 mt-2"
                            drop-placeholder="Drop file here..."
                            placeholder="Choose a file or drop it here..."
                            size="lg"
                            v-model="file"
                    ></b-form-file>
                    <div class="mt-3">Selected file: {{ file ? file.name : '' }}</div>
                </b-form-group>
                <b-button @click.prevent="crop1" variant="success">crop</b-button>
                <b-button @click.prevent="undo1" variant="warning">Undo</b-button>
                <b-button @click.prevent="redo1" variant="primary">Redo</b-button>
                <b-button @click.prevent="reset1" variant="danger">Reset</b-button>

                <b-button @click.prevent="crop2" variant="success">crop</b-button>
                <b-button @click.prevent="undo2" variant="warning">Undo</b-button>
                <b-button @click.prevent="redo2" variant="primary">Redo</b-button>
                <b-button @click.prevent="reset2" variant="danger">Reset</b-button>
            </b-col>
        </b-row>
    </div>
</template>
<script>
	import polygonCrop from 'PolygonCropper.umd';
	// import polygonCrop from 'vue-polygon-cropper';
import Vue from 'vue';
import {BootstrapVue, IconsPlugin} from 'bootstrap-vue';

Vue.config.productionTip = false;
Vue.use(BootstrapVue);
Vue.use(IconsPlugin);

	export default {
		name: 'App',
		data() {
			return {
				imgSrc1: '/demo.png',
				imgSrc2: '/demo.png',
				file: null,
				show1: false,
				showResult1: false,
				show2: false,
				showResult2: false,
				showPointer: true,
				resultImage1: "",
				resultImage2: ""

			};
		},
		components: {
			polygonCrop
		},
		methods: {
			setImage(e) {
				const file = e.target.files[0];
				if (!file && file.type.indexOf('image/') === -1) {
					alert('Please select an image file');
					return;
				}
				if (typeof FileReader === 'function') {
					const reader = new FileReader();
					reader.onload = (event) => {
						this.imgSrc1 = event.target.result;
						this.imgSrc2 = event.target.result;
						this.show1 = true;
						this.show2 = true;
					};
					reader.readAsDataURL(file);
				} else {
					alert('Sorry, FileReader API not supported');
				}
			},
crop2: function () {
    this.$refs.canvas2.crop();
    this.resultImage2 = this.$refs.canvas2.resultImage;
    this.show2 = false;
    this.showResult2 = true;
},
			crop1: function () {
				this.$refs.canvas1.crop();
				this.resultImage1 = this.$refs.canvas1.resultImage;
				this.show1 = false;
				this.showResult1 = true;
			},
			undo1: function () {
				this.$refs.canvas1.undo();
			},
			redo1: function () {
				this.$refs.canvas1.redo();
			},
			reset1: function () {
				this.show1 = true;
				this.showResult1 = false;
				this.$refs.canvas1.reset();
			},
			undo2: function () {
				this.$refs.canvas2.undo();
			},
			redo2: function () {
				this.$refs.canvas2.redo();
			},
			reset2: function () {
				this.show2 = true;
				this.showResult2 = false;
				this.$refs.canvas2.reset();
			}
		}
	};
</script>

<style>
@import '~bootstrap/dist/css/bootstrap.css';
@import '~bootstrap-vue/dist/bootstrap-vue.css';

    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .some-class {
        border: 1px solid #2c3e50;
    }
</style>
