<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";

const posts = ref([]);
const postsName = ref("");

// .GET - getting all the data
const getAllData = async () => {
  try {
    const resp = await axios.get("https://jsonplaceholder.typicode.com/posts");
    // console.log(resp);
    posts.value = resp.data;
  } catch (err) {
    console.log(err);
  }
};

// .POST sending the data
const addPost = async () => {
  const resp = await axios.post("https://jsonplaceholder.typicode.com/posts", {
    title: postsName.value,
  });

  posts.value = [resp.data, ...posts.value];
  console.log(posts.value);
  postsName.value = "";
};

// .PATHC updating specific object data when it done send the data

const boughtPosts = async (id) => {
  try {
    await axios.patch("https://jsonplaceholder.typicode.com/posts/$%7Bid%7D%60", {
      boughtPost: true,
    });
  } catch (err) {
    console.log(err);
  }
};

const removePost = async (id) => {
  axios.delete("https://jsonplaceholder.typicode.com/posts/${id}");
  posts.value = posts.value.filter((item) => item.id !== id);
};

onMounted(() => {
  getAllData();
});
</script>

<template>
  <div class="min-h-screen bg-gray-200">
    <div class="flex justify-center pt-20">
      <h1 class="font-bold text-green-600 text-3xl">Vue Axios</h1>
    </div>

    <div class="flex justify-center mt-6">
      <input
        class="border border-gray-400 p-1"
        type="text"
        @keydown.enter="addPost"
        placeholder="type something ..."
        v-model="postsName"
      />
      <div class="ml-2">
        <button @click.prevent="addPost" class="bg-green-400 hover:bg-green-300 text-white rouded-md px-3 py-2">
          Add Post
        </button>
      </div>
    </div>

    <div class="mx-auto w-[50%] mt-10">
      <ul>
        <li class="cursor-pointer" v-for="post in posts" :key="post.id" @click.prevent="boughtPosts(post.id)">
          <h1 @dblclick.prevent="removePost(post.id)">{{ post.title }}</h1>
        </li>
      </ul>
    </div>
  </div>
</template>
