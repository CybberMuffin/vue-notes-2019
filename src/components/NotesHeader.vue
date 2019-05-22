<template>
  <h2>{{title}}</h2>
</template>

<script>
export default {
  name: "NotesHeader",
  props: {
    title: {
      type: String,
      default: "Default title"
    }
  }
};
</script>

<style lang="scss" scoped>
h2 {
  text-align: center;
  font-weight: 500;
  color: grey;
  text-shadow: 0 2px 3px rgba(255, 255, 255, 0.5);
}
</style>