<template>
  <div class="container">
    <span v-show="filter" class="search">
      Search for hashtag: {{tag}}
      <a
        @click="filter = false"
        style="color: red; cursor: pointer;"
      >Return</a>
    </span>
    <div class="notes-grid" ref="grid">
      <note-item v-for="note in filteredNotes" :key="note.messageID" :note="note"/>
    </div>
  </div>
</template>

<script>
import Masonry from "masonry-layout";
import NoteItem from "@/components/NoteItem.vue";

export default {
  name: "notes-grid",

  data: () => ({
    filter: false,
    tag: null
  }),

  props: {
    notes: {
      type: Array,
      required: true
    }
  },

  components: {
    NoteItem
  },

  methods: {
    doTagSearch(tag) {
      this.filter = true;
      this.tag = tag;
    }
  },

  computed: {
    filteredNotes() {
      return this.filter
        ? this.notes.filter(item => item.text.includes(this.tag))
        : this.notes;
    }
  },

  mounted() {
    const grid = this.$refs.grid;
    this.msnry = new Masonry(grid, {
      itemSelector: ".note",
      columnWidth: 200,
      gutter: 10,
      isFitWidth: true
    });
  },

  updated() {
    this.msnry.reloadItems();
    this.msnry.layout();
  }
};
</script>

<style lang="scss" scoped>
.notes-grid {
  margin: 0 auto;
}
.search {
  margin: 0 auto;
  text-align: center;
  font-size: 18px;
  margin-bottom: 30px;
}
</style>