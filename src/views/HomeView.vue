<template>
  <div class="h-screen w-screen flex items-center justify-center flex-col">
    <div class="text-center">
      <h1 class="text-3xl text-emerald-500 mb-10">LINK SHORTENER</h1>
    </div>
    <div class="flex items-center justify-center mb-5">
      <input
        class="border-2 border-gray-300 bg-white h-10 px-2 py-2 text-md md:text-lg text-center rounded-lg focus:outline-none"
        placeholder="Enter the link you want to shorten"
        type="text"
        :value="value"
        @input="(event) => (link = event.target.value)"
      />
      <button
        class="rounded-lg bg-emerald-500 p-2 ml-6 text-center text-md md:text-lg text-white hover:bg-emerald-600 hover:text-white hover:shadow-lg transition duration-200 ease-in-out"
        @click="getUrl"
      >
        Shorten It!
      </button>
    </div>
    <div>
      <h1
        v-if="isError"
        class="text-white text-lg bg-red-800 px-10 md:px-20 py-2 rounded-lg text-center"
      >
        Please enter a valid Link
      </h1>
    </div>
    <div
      v-if="shortenedLink != ''"
      class="mt-5 text-md md:text-3xl text-center"
    >
      <h1 class="text-white">Your shortened link:</h1>
      <div class="flex justify-center items-center">
        <a target="_blank" class="text-blue-500" :href="shortenedLink">{{
          shortenedLink
        }}</a>
        <i
          @click="copyUrl(shortenedLink)"
          class="fa fa-copy ml-2 text-white text-sm cursor-pointer"
        ></i>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "HomeView",

  data() {
    return {
      shortenedLink: "",
      value: "",
      link: "",
      isError: false,
    };
  },
  mounted() {},
  methods: {
    getUrl() {
      this.isError = false;
      this.shortenedLink = "";
      axios

        .get("https://api.shrtco.de/v2/shorten?url=" + this.link)
        .then(
          (response) =>
            (this.shortenedLink = response.data.result.full_short_link)
        )
        .catch(() => {
          this.isError = true;
        })
        .finally(() => {
          this.link = "";
        });
    },
    async copyUrl(url) {
      try {
        await navigator.clipboard.writeText(url);
        alert("Copied to clipboard");
      } catch (error) {
        alert("Failed to copy");
      }
    },
  },
};
</script>
