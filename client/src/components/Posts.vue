<template>
  <div>
    <h1>Posts</h1>
    <input
      type="text"
      v-model="title"
      placeholder="Title"
      class="title-input input"
    >
    <input
      type="text"
      v-model="body"
      placeholder="Body"
      class="body-input input"
    >

    <button v-if="isEditing" @click="updatePost">Update</button>
    <button v-if="isEditing" @click="cancelEdit">Cancel</button>
    <button v-else @click="createPost">Create</button>
  </div>
</template>

<script>
export default {
  name: "Posts",
  data() {
    return {
      title: "",
    }
  },
  methods: {
    updatePost() {
      return true;
    },
    cancelEdit() {
      return true;
    },
  },
}
</script>

<style scoped>
.input {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  color: #111;
  border: 2px solid #ccc;
  border-radius: 4px;
  background-color: #f8f8f8;
  box-sizing: border-box;
  resize: vertical;
}
</style>
