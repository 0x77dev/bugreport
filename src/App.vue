<template>
  <div
    id="app"
    class="flex items-center justify-center w-full min-h-screen antialiased font-mono"
  >
    <div class="w-full">
      <h1 class="text-center text-4xl">Bug Report</h1>
      <form
        class="px-4 mt-4 md:max-w-lg md:mx-auto w-full"
        @submit.prevent="submitForm"
      >
        <div
          :class="{
            'bg-green-100 border-green-400 text-green-700': success,
            'bg-red-100 border-red-400 text-red-700': error,
            'bg-blue-100 border-blue-400 text-blue-700': !success && !error,
          }"
          class="h-32 md:h-auto text-center transition mb-4 border px-4 py-3 rounded flex flex-col items-center justify-center w-full"
          role="alert"
        >
          <strong class="font-bold" v-if="success">Task added!</strong>
          <strong class="font-bold" v-if="error">Something went wrong</strong>
          <strong class="font-bold" v-if="!success && !error"
            >What is this?</strong
          >
          <span v-if="success">Your report was sent successfully</span>
          <span v-if="error"
            >There was an error when adding your task, check logs.</span
          >
          <span v-if="!error && !success"
            >It is an bug report form, please describe your problem
            clearly</span
          >
        </div>
        <label class="block mb-4">
          <span class="text-gray-700">Title</span>
          <input
            v-model="title"
            type="text"
            class="form-input mt-1 block w-full"
            required
          />
        </label>
        <label class="block mb-4">
          <span class="text-gray-700">Report Content</span>
          <textarea
            v-model="content"
            style="min-height: 500px"
            class="form-textarea mt-1 block w-full"
            rows="3"
            required
          ></textarea>
        </label>
        <div class="text-center">
          <button
            type="submit"
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          >
            Submit
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "app",
  data: () => ({
    title: null,
    content: `
Project Name: 
Problem: 
Steps to reproduce: 
1. Open something
2. click something
3. receive an error
    `,
    success: false,
    error: false,
  }),
  methods: {
    submitForm() {
      axios
        .post("/.netlify/functions/send-to-things", {
          title: this.title,
          content: this.content,
        })
        .then(() => {
          this.success = true;
          this.title = this.content = null;

          setTimeout(() => {
            this.success = false;
          }, 3000);
        })
        .catch(() => {
          this.error = true;

          setTimeout(() => {
            this.error = false;
          }, 3000);
        });
    },
  },
};
</script>

<style lang="less">
@tailwind base;
@tailwind components;
@tailwind utilities;

.transition {
  transition: color 2s ease-in-out, background-color 2s ease-in-out,
    border-color 2s ease-in-out;
}
</style>
