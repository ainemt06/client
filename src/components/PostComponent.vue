<template>
  <div class="container">
    <h1 class="title">Latest Posts</h1>

    <!-- Create Post Section -->
    <div class="create-post">
      <label for="create-post" class="create-post-label">Post Something!</label>
      <input
        type="text"
        id="create-post"
        v-model="text"
        placeholder="Create a post"
        class="create-post-input"
      />
      <button @click="createPost" class="create-post-btn">Post!</button>
    </div>

    <hr />

    <!-- Error Message -->
    <p class="error" v-if="error">{{ error }}</p>

    <!-- Posts List -->
    <div class="posts-container">
      <div
        class="post"
        v-for="post in posts"
        :key="post._id"
        @dblclick="deletePost(post._id)"
      >
        <div class="post-date">
          {{ `${post.createdAt.getMonth() + 1}/${post.createdAt.getDate()}/${post.createdAt.getFullYear()}` }}
        </div>
        <p class="post-text">{{ post.text }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from '../PostService';

export default {
  name: 'PostComponent',
  data() {
    return {
      posts: [],
      error: '',
      text: ''
    };
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch (err) {
      this.error = err.message;
    }
  },
  methods: {
    async createPost() {
      await PostService.insertPost(this.text);
      this.posts = await PostService.getPosts();
    },
    async deletePost(id) {
      await PostService.deletePost(id);
      this.posts = await PostService.getPosts();
    }
  }
};
</script>

<style scoped>
/* Container */
.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #f4f4f4;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

/* Title */
.title {
  text-align: center;
  font-size: 2rem;
  color: #333;
  margin-bottom: 1.5rem;
}

/* Create Post Section */
.create-post {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 2rem;
}

.create-post-label {
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
  color: #444;
}

.create-post-input {
  width: 80%;
  padding: 0.8rem;
  margin-bottom: 1rem;
  border-radius: 8px;
  border: 1px solid #ddd;
  font-size: 1rem;
}

.create-post-btn {
  background-color: #4caf50;
  color: white;
  padding: 0.7rem 1.5rem;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.create-post-btn:hover {
  background-color: #45a049;
}

/* Error Message */
.error {
  background-color: #f8d7da;
  border: 1px solid #f5c6cb;
  color: #721c24;
  padding: 0.8rem;
  border-radius: 4px;
  margin-bottom: 2rem;
}

/* Posts Container */
.posts-container {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

/* Individual Post */
.post {
  background-color: #ffffff;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s, box-shadow 0.3s;
}

.post:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

/* Post Date */
.post-date {
  font-size: 0.9rem;
  color: #888888;
  text-align: right;
  margin-bottom: 1rem;
}

/* Post Text */
.post-text {
  font-size: 1.1rem;
  line-height: 1.6;
  color: #333;
}

/* Responsive Design */
@media (max-width: 600px) {
  .create-post-input {
    width: 100%;
  }

  .create-post-btn {
    width: 100%;
    font-size: 1.2rem;
  }

  .container {
    padding: 1rem;
  }

  .title {
    font-size: 1.6rem;
  }
}
</style>
