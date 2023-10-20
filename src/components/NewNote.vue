<!-- eslint-disable vue/no-mutating-props -->
<template>
  <section class="new-note">
    <form>
      <label for="title">Title</label>
      <input v-model="note.title" type="text" id="title" />
      <label for="priority">Priority</label>
      <select v-model="note.priority" id="priority">
        <option>ordinary</option>
        <option>important</option>
        <option>essential</option>
      </select>
      <label for="description">Description</label>
      <textarea v-model="note.description" id="description"></textarea>
      <button type="submit" class="btn btnPrimary" @click.prevent="addNote">
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