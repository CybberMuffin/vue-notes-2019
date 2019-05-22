<template>
  <div class="note-editor">
    <div>
      <textarea v-model="text" class="form-input" placeholder="Enter your note here ..." row="5"/>
      <button class="add-button" @click="createNote" :disabled="text.trim().length == 0">Add</button>
      <div v-show="!opened" @click="openPicker">
        <div
          class="color-block"
          v-b-tooltip.hover.right
          title="Click to change colour of your note"
          :style="{backgroundColor: colors.hex}"
        ></div>
      </div>
      <div v-show="opened">
        <chrome-picker class="picker" v-model="colors"></chrome-picker>
        <button class="add-button save-button" @click="openPicker">minimize</button>
      </div>
    </div>
  </div>
</template>

<script>
import { Chrome } from "vue-color";

function setFore(background) {
  let o = Math.round(
    (parseInt(background.r) * 299 +
      parseInt(background.g) * 587 +
      parseInt(background.b) * 114) /
      1000
  );

  return o > 125 ? "black" : "white";
}

const rnd = () => Math.floor(Math.random() * 255);

export default {
  name: "notes-editor",

  data: () => ({
    opened: false,
    text: "",
    colors: {
      hex: "#194d33",
      hsl: { h: 150, s: 0.5, l: 0.2, a: 1 },
      hsv: { h: 150, s: 0.66, v: 0.3, a: 1 },
      rgba: { r: 25, g: 77, b: 51, a: 1 },
      a: 1
    }
  }),

  components: {
    "chrome-picker": Chrome
  },

  methods: {
    createNote() {
      let localBg = {
        r: this.colors.rgba.r,
        g: this.colors.rgba.g,
        b: this.colors.rgba.b
      };

      const note = {
        id: new Date().getTime(),
        text: this.text,
        txtcolor: setFore(localBg),
        bgcolor: `rgb(${localBg.r}, ${localBg.g}, ${localBg.b})`
      };
      this.$emit("createNote", note);
      this.text = "";
    },
    openPicker() {
      this.opened = !this.opened;
    }
  }
};
</script>

<style lang="scss" scoped>
.note-editor {
  width: 100%;
  max-width: 600px;
  padding: 16px;
  margin: 16px auto;
  background-color: white;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  border-radius: 2px;
  display: flex;
  flex-direction: column;
}

textarea {
  width: 100%;
  resize: none;
  margin: 5px;
  font-size: 18px;
  border: none;
  font-weight: 300;
}

textarea:focus {
  outline: 0;
}

.add-button {
  align-self: flex-end;
  width: 100px;
  background-color: #44c767;
  border-radius: 8px;
  border: 1px solid #18ab29;
  cursor: pointer;
  color: #ffffff;
  font-size: 14px;
  padding: 8px 8px;
  text-transform: uppercase;
  text-decoration: none;
  text-shadow: 0px 1px 0px #2f6627;
}

.add-button:hover {
  background-color: #5cbf2a;
}

.add-button:active {
  position: relative;
  top: 1px;
}

.add-button:focus {
  outline: 0;
}

.color-block {
  width: 20px;
  height: 20px;
  margin-top: 20px;
  border-radius: 6px;
  display: inline-block;
}

.save-button {
  display: inline-block;
}

.picker {
  margin: 10px 0;
}
</style>