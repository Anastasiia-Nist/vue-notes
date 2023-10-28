<!-- eslint-disable vue/no-mutating-props -->
<template>
  <section class="new-note">
    <form @submit.prevent="addNote">
      <label for="title">Title</label>
      <input v-model="note.title" type="text" id="title" />
      <label for="priority">Priority</label>
      <select v-model="note.selected" id="priority">
        <option v-for="(priority, index) in note.priorities" :key="index">
          {{ priority.alias }}
        </option>
      </select>
      <label for="description">Description</label>
      <textarea v-model="note.description" id="description"></textarea>
      <button type="submit" class="btn btnPrimary">
        New Note
      </button>
    </form>
  </section>
</template>

<script>
export default {
  props: {
    note: {
      type: Object,
      requires: true,
    },
  },
  methods: {
    addNote() {
      this.$emit("addNote", this.note);
    },
  },
};
</script>
<style lang="scss">
.new-note {
  text-align: center;
  select {
    width: 60%;
    padding: 16px 26px;
    margin-bottom: 30px;
    border-radius: 20px;
  }
}
</style>