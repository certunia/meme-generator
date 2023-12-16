<script>
import VueDrawingCanvas from "vue-drawing-canvas";

export default {
  name: "ServeDev",
  components: {
    VueDrawingCanvas,
  },
  data() {
    return {
      initialImage: [
        {
          type: "dash",
          from: {
            x: 262,
            y: 154,
          },
          coordinates: [],
          color: "#000000",
          width: 5,
          fill: false,
        },
      ],
      x: 0,
      y: 0,
      image: "",
      eraser: false,
      fillShape: false,
      line: 5,
      color: "#000000",
      strokeType: "dash",
      lineCap: "round",
      lineJoin: "round",
      backgroundColor: "#FFFFFF",
      backgroundImage: null,
      watermark: null,
      additionalImages: [],
    };
  },
  mounted() {
    if ("vue-drawing-canvas" in window.localStorage) {
      this.initialImage = JSON.parse(
          window.localStorage.getItem("vue-drawing-canvas")
      );
    }
  },
  methods: {
    async setImage(event) {
      let URL = window.URL;
      this.backgroundImage = URL.createObjectURL(event.target.files[0]);
      await this.$refs.VueCanvasDrawing.redraw();
    },
    async setWatermarkImage(event) {
      let URL = window.URL;
      this.watermark = {
        type: "Image",
        source: URL.createObjectURL(event.target.files[0]),
        x: 0,
        y: 0,
        imageStyle: {
          width: 600,
          height: 400,
        },
      };
      await this.$refs.VueCanvasDrawing.redraw();
    },
    getCoordinate(event) {
      let coordinates = this.$refs.VueCanvasDrawing.getCoordinates(event);
      this.x = coordinates.x;
      this.y = coordinates.y;
    },
    getStrokes() {
      window.localStorage.setItem(
          "vue-drawing-canvas",
          JSON.stringify(this.$refs.VueCanvasDrawing.getAllStrokes())
      );
      alert(
          "Strokes saved, reload your browser to see the canvas with previously saved image"
      );
    },
    removeSavedStrokes() {
      window.localStorage.removeItem("vue-drawing-canvas");
      alert("Strokes cleared from local storage");
    },
  },
};
</script>

<template>
  <div id="app">
    <div class="flex-row">
      <div class="source">
        <p>Canvas:</p>
        <vue-drawing-canvas
            ref="VueCanvasDrawing"
            v-model:image="image"
            :width="600"
            :height="400"
            :stroke-type="strokeType"
            :line-cap="lineCap"
            :line-join="lineJoin"
            :fill-shape="fillShape"
            :lineWidth="line"
            :color="color"
            :background-color="backgroundColor"
            :background-image="backgroundImage"
            :watermark="watermark"
            :initial-image="initialImage"
            saveAs="png"
            :styles="{
              border: 'solid 1px #000',
            }"
            @mousemove="getCoordinate($event)"
            :additional-images="additionalImages"
        />
        <div class="button-container">
          <button type="button" @click.prevent="$refs.VueCanvasDrawing.undo()">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-arrow-counterclockwise"
                viewBox="0 0 16 16"
            >
              <path
                  fill-rule="evenodd"
                  d="M8 3a5 5 0 1 1-4.546 2.914.5.5 0 0 0-.908-.417A6 6 0 1 0 8 2v1z"
              />
              <path
                  d="M8 4.466V.534a.25.25 0 0 0-.41-.192L5.23 2.308a.25.25 0 0 0 0 .384l2.36 1.966A.25.25 0 0 0 8 4.466z"
              />
            </svg>
            Undo
          </button>
          <button type="button" @click.prevent="$refs.VueCanvasDrawing.redo()">
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-arrow-clockwise"
                viewBox="0 0 16 16"
            >
              <path
                  fill-rule="evenodd"
                  d="M8 3a5 5 0 1 0 4.546 2.914.5.5 0 0 1 .908-.417A6 6 0 1 1 8 2v1z"
              />
              <path
                  d="M8 4.466V.534a.25.25 0 0 1 .41-.192l2.36 1.966c.12.1.12.284 0 .384L8.41 4.658A.25.25 0 0 1 8 4.466z"
              />
            </svg>
            Redo
          </button>
          <!--<button-->
          <!--    type="button"-->
          <!--    @click.prevent="$refs.VueCanvasDrawing.redraw()"-->
          <!--&gt;-->
          <!--  <svg-->
          <!--      xmlns="http://www.w3.org/2000/svg"-->
          <!--      width="16"-->
          <!--      height="16"-->
          <!--      fill="currentColor"-->
          <!--      class="bi bi-arrow-repeat"-->
          <!--      viewBox="0 0 16 16"-->
          <!--  >-->
          <!--    <path-->
          <!--        d="M11.534 7h3.932a.25.25 0 0 1 .192.41l-1.966 2.36a.25.25 0 0 1-.384 0l-1.966-2.36a.25.25 0 0 1 .192-.41zm-11 2h3.932a.25.25 0 0 0 .192-.41L2.692 6.23a.25.25 0 0 0-.384 0L.342 8.59A.25.25 0 0 0 .534 9z"-->
          <!--    />-->
          <!--    <path-->
          <!--        fill-rule="evenodd"-->
          <!--        d="M8 3c-1.552 0-2.94.707-3.857 1.818a.5.5 0 1 1-.771-.636A6.002 6.002 0 0 1 13.917 7H12.9A5.002 5.002 0 0 0 8 3zM3.1 9a5.002 5.002 0 0 0 8.757 2.182.5.5 0 1 1 .771.636A6.002 6.002 0 0 1 2.083 9H3.1z"-->
          <!--    />-->
          <!--  </svg>-->
          <!--  Refresh-->
          <!--</button>-->
          <!--<button type="button" @click.prevent="$refs.VueCanvasDrawing.reset()">-->
          <!--  <svg-->
          <!--      xmlns="http://www.w3.org/2000/svg"-->
          <!--      width="16"-->
          <!--      height="16"-->
          <!--      fill="currentColor"-->
          <!--      class="bi bi-file-earmark"-->
          <!--      viewBox="0 0 16 16"-->
          <!--  >-->
          <!--    <path-->
          <!--        d="M14 4.5V14a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V2a2 2 0 0 1 2-2h5.5L14 4.5zm-3 0A1.5 1.5 0 0 1 9.5 3V1H4a1 1 0 0 0-1 1v12a1 1 0 0 0 1 1h8a1 1 0 0 0 1-1V4.5h-2z"-->
          <!--    />-->
          <!--  </svg>-->
          <!--  Reset-->
          <!--</button>-->
        </div>
        <div class="button-container">
          <select v-model="line">
            <option v-for="n in 25" :key="'option-' + n" :value="n">
              {{ n }}
            </option>
          </select>
          <input type="color" v-model="color" />
          <select v-model="strokeType">
            <option value="dash">Dash</option>
            <option value="line">Straight Line</option>
            <option value="circle">Circle</option>
            <option value="square">Square</option>
            <option value="triangle">Triangle</option>
            <option value="half_triangle">Half Triangle</option>
          </select>
          <button type="button" @click.prevent="fillShape = !fillShape">
            <span v-if="fillShape">
              <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-square-fill"
                  viewBox="0 0 16 16"
              >
                <path
                    d="M0 2a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V2z"
                />
              </svg>
              Fill
            </span>
            <span v-else>
              <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-square"
                  viewBox="0 0 16 16"
              >
                <path
                    d="M14 1a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1H2a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1h12zM2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2H2z"
                />
              </svg>
              Stroke
            </span>
          </button>
        </div>
        <div class="button-container">
          <div style="margin-right: 30px">
            <p style="margin-bottom: 0">Background Color:</p>
            <input type="color" v-model="backgroundColor" />
          </div>
          <div>
            <p style="margin-bottom: 0">Upload Background Image:</p>
            <input type="file" @change="setImage($event)" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@1,700&display=swap");
body {
  font-family: "Roboto", sans-serif;
}
.flex-row {
  display: flex;
  flex-direction: row;
}
.button-container {
  display: flex;
  flex-direction: row;
}
.button-container > * {
  margin-top: 15px;
  margin-right: 10px;
}
</style>
