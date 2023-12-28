<template>
  <div>
    <h1>Posts</h1>
    <input
      type="text"
      v-model="title"
      placeholder="Title"
      class="title-input input"
    >
    <textarea v-model="body" placeholder="Body" class="body-input input" rows="4" cols="50"></textarea>

    <button class="btn" v-if="isEditing" @click="updatePost">Update</button>
    <button class="btn" v-if="isEditing" @click="cancelEdit">Cancel</button>
    <button class="btn" v-else @click="createPost">Create</button>

    <ul class="posts">
      <li
        v-for="post in posts"
        :key="post.id"
        class="post"
      >
      
        <h2 class="post__title">{{ post.title }}</h2>
        <hr />
        <p>{{ post.body }}</p>

        <p class="post__date">{{ post.created_at }}</p>

        <div class="post__footer">
          <button>Edit</button>
          <button @click="deletePost(post.id)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Posts",
  data() {
    return {
      title: "",
      body: "",
      isEditing: false,
      API_URL: "http://localhost:3000/posts",
      posts: [],
    }
  },
  created() {
    this.getPosts();
  },
  methods: {
    updatePost() {
      return true;
    },
    cancelEdit() {
      return true;
    },
    async getPosts() {
      const data = await fetch(this.API_URL);
      this.posts = await data.json();
    },
    async createPost() {
      if (!this.title.length || !this.body.length) return;

      const data = await fetch(this.API_URL, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          title: this.title,
          body: this.body,
        }),
      })

      if(data.ok) alert('Post added!')
      if(!data.ok) alert('Something that wrong!')
    },
    async deletePost(id) {
      await fetch(`${this.API_URL}/${id}`, {
        method: "DELETE",
      })

      // this.posts.filter(post => post.id !== id) // And reload after filtering? refs?
      this.getPosts();
    }
  },
}
</script>

<style lang="scss" scoped>
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

.btn {
  width: 100%;
  margin-top: 20px;
}
.posts {
  list-style: none;
  padding: 0;
  margin-top: 20px;
}

.post {
  text-align: left;
  background-color: #313030;
  padding: 10px;
  margin-bottom: 15px;
  border-radius: 6px;

  &__title {
    margin-top: 0;
    margin-bottom: 10px;
  }

  &__date {
    text-align: end;
  }

  &__footer {
    display: flex;
    justify-content: flex-end;

    button {
      margin-left: 10px;
    }
  }
}
</style>
