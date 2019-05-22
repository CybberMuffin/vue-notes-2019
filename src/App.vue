<template>
  <div class="notes-app">
    <notes-header title="Kovalenko Dmytro Note App"/>
    <notes-header title="Double click on note to edit it, enter or double click to save changes"/>
    <notes-editor @createNote="createNote" @deleteNote="deleteNote"/>
    <notes-grid :notes="notes"/>
  </div>
</template>

<script>
import NotesHeader from "@/components/NotesHeader.vue";
import NotesEditor from "@/components/NotesEditor.vue";
import NotesGrid from "@/components/NotesGrid.vue";
import NotesSearch from "@/components/NotesSearch.vue";

import { bus } from "../src/bus";

const notes = localStorage.notes ? JSON.parse(localStorage.notes) : [];

export default {
  name: "notes-app",

  components: {
    NotesHeader,
    NotesEditor,
    NotesGrid,
    NotesSearch
  },

  data: () => ({
    title: "Notes App2",
    notes
  }),

  methods: {
    createNote(note) {
      this.notes.push(note);
      this.updateStorage();
    },

    deleteNote(id) {
      this.notes = this.notes.filter(item => item.id != id);
      this.updateStorage();
    },

    setChange(id, text) {
      this.notes[this.notes.findIndex(note => note.id == id)].text = text;
      this.updateStorage();
    },

    updateStorage() {
      localStorage.notes = JSON.stringify(this.notes);
    }
  },

  created() {
    bus.$on("editText", data => {
      console.log(data["id"]);
      console.log(data["text"]);
      this.notes[this.notes.findIndex(note => note.id == data["id"])].text =
        data["text"];
      this.updateStorage();
    });
  }
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  font-weight: 300;
  background-color: #eaeaea;
}

.notes-app {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 980px;
  margin: 0 auto;
}
</style>
