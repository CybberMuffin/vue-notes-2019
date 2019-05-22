<template>
  <div
    class="note"
    :style="{ backgroundColor: note.bgcolor, color: note.txtcolor, borderRadius: borderRad}"
    @dblclick="editNote"
  >
    <span class="delete-note" @click="$parent.$parent.deleteNote(note.id)">x</span>
    <div
      :id="'text-' + this.note.id"
      class="area"
      :contenteditable="editable"
      :style="{ color: note.txtcolor}"
      v-html="hashedText"
      @click="linkService"
      @keyup.enter="editNote"
    ></div>
  </div>
</template>



<script>
import { bus } from "../bus";
import { constants } from "crypto";
export default {
  name: "note-item",

  data: () => ({
    editable: false,
    borderRad: "2px"
  }),

  props: {
    note: {
      type: Object,
      required: true
    }
  },

  methods: {
    editNote() {
      if (this.editable) {
        const newText = document.getElementById(`text-${this.note.id}`)
          .innerText;
        if (newText.trim().length == 0) return;
        bus.$emit("editText", {
          id: this.note.id,
          text: newText
        }); // решил попробовать, как работает bus
        this.borderRad = "2px";
      } else {
        this.borderRad = "20px";
      }
      this.editable = !this.editable;
    },

    linkService(event) {
      const element = event.target;
      if (element.classList.contains("hashlink")) {
        const hashtag = element.innerHTML.trim();
        this.$parent.doTagSearch(hashtag);
      }
    },

    clickTextHandler(event) {
      const element = event.target;
      if (element.classList.contains("hashtag")) {
        const hash = element.innerHTML.trim().substr(1);
        this.$emit("tag-clicked", hash);
      }
    }
  },

  computed: {
    hashedText() {
      return this.note.text.replace(
        /#([\w]+)/g,
        '<a href="#" class="hashlink" style="color: #1E90FF">#$1</a>'
      );
    }
  }
};
</script>

<style lang="scss" scoped>
.note {
  width: 200px;
  height: auto;
  float: left;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  border-radius: 2px;
  padding: 10px;
  margin-bottom: 10px;
  transition: box-shadow 0.3s;
  word-wrap: break-word;
  position: relative;
  font-size: 18px;
}

.note:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
}

.delete-note {
  position: absolute;
  top: 5px;
  right: 5px;
  display: none;
  color: rgba(0, 0, 0, 0.6);
  cursor: pointer;
}

.note:hover .delete-note {
  display: block;
}

.area {
  background: transparent;
  border: none;
  outline: none;
  resize: none;
  font-size: 18px;
  cursor: pointer;
}

.add-button {
  align-self: flex-end;
  width: 60px;
  background-color: #44c767;
  border-radius: 6px;
  border: 1px solid #18ab29;
  cursor: pointer;
  color: #ffffff;
  font-size: 12px;
  padding: 6px 6px;
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
</style>