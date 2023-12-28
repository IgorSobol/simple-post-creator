<template>
  <div>
    <div class="create-post-form">
      <h1>Posts</h1>
      <input
        type="text"
        v-model="title"
        placeholder="Title"
        class="title-input input"
      >
      <textarea v-model="body" placeholder="Body" class="body-input input" rows="4" cols="50"></textarea>

      <div class="create-post-form__footer">
        <div v-if="isEditing" class="create-post-form__footer-edit-group">
          <button class="btn" @click="updatePost">Update</button>
          <button class="btn" @click="cancelEdit">Cancel</button>
        </div>

        <button class="btn" v-else @click="createPost">Create</button>
      </div>
    </div>

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
          <button @click="editPost(post.id)">Edit</button>
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
      editPostId: "",
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
    editPost(id) {
      const post = this.posts.filter(post => post.id === id);

      this.title = post[0].title;
      this.body = post[0].body;
      this.editPostId = id;
      this.isEditing = true;

      window.scrollTo({
        top: 0,
        behavior: "smooth",
      })
    },
    cancelEdit() {
      this.title = "";
      this.body = "";
      this.isEditing = false;
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

      if(data.ok) {
        alert('Post editing!')
        this.cancelEdit();
        this.getPosts();
      }
      if(!data.ok) alert('Something that wrong!')
    },
    async deletePost(id) {
      await fetch(`${this.API_URL}/${id}`, {
        method: "DELETE",
      })

      // this.posts.filter(post => post.id !== id) // And reload after filtering? refs?
      this.getPosts();
    },
    async updatePost() {
      if (!this.title.length || !this.body.length) return;

      const data = await fetch(`${this.API_URL}/${this.editPostId}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          title: this.title,
          body: this.body,
          id: this.editPostId,
        }),
      })

      if(data.ok) {
        alert('Post editing!')
        this.cancelEdit();
        this.getPosts();
      }
      if(!data.ok) alert('Something that wrong!')
    },
  },
}
</script>

<style lang="scss" scoped>
.create-post-form__footer {
  &-edit-group {
    display: flex;
    justify-content: space-between;

    button {
      &:first-child {
        margin-right: 10px;
      }
      &:last-child {
        margin-left: 10px;
      }
    }
  }
}

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
