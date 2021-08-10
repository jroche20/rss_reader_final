<template>
  <div id="app">
    <form @submit.prevent="getRss">
      <div>
        <label> rss url</label>
        <br />
        <input v-model="rssUrl" />
      </div>
      <input type="submit" />
    </form>
    <div v-for="item of items" :key="item.title">
      <h1>{{ item.title }}</h1>
      <p>{{ item.author }}</p>
      <a :href="item.link">{{ item.link }}</a>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      rssUrl: "",
      items: [],
    };
  },
  methods: {
    async getRss() {
      const urlRegex = /^(http|https):\/\/[w\d]+\.[\w](\/[\w\d]+)?/;
      if (!urlRegex.test(this.rssUrl)) {
        console.log("not valid");
        return;
      }
      console.log("valid");
      const res = await fetch(
        `https://api.allorigins.win/get?url=${this.rssUrl}`
      );
      const { contents } = await res.json();
      const feed = new window.DOMParser().parseFromString(contents, "text/xml");
      const items = feed.querySelectorAll("title");
      this.items = [...items].map((el) => ({
        // link: el.querySelector("url"),
        title: el.querySelector("title"),
        // author: el.querySelector("name"),
      }));
      console.log(feed);
      console.log(this.items);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
