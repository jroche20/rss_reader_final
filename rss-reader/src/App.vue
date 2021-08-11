<template>
  <div id="app">
    <img src="@/assets/caena.svg" alt="caena">
    <form @submit.prevent="getRss">
      <div>
        <label> rss url</label>
        <br />
        <input v-model="rssUrl" class="url_input" placeholder="url"/>
      </div>
      <input type="submit" class="submit"/>
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
      const items = feed.querySelectorAll("head");
      console.log(feed);
      this.items = [...items].map((el) => ({
        // link: el.querySelector('meta[property="og:url"]'),
        link: el.querySelector('link[rel="canonical"]').getAttribute('href'),
        title: el.querySelector("title").innerHTML,
        // author: el.querySelector("author").innerHTML,
      }));
      console.log(this.items);
    },
  },
};
</script>

<style>
#app {
  /* @import "./scss/main.scss"; */
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500&display=swap');
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  font-family: 'Poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  line-height: 30px;
}

.submit {
  font-family: 'Poppins', sans-serif;
  border: 0.5px solid #333E49; 
  background-color: #1D817F;
  color: white;
  width: 100px;
  padding: 5px;
  font-size: 16px;
}

form {
  background-color: rgba(246, 247, 249, 0.5);
  padding: 20px;
  margin: 40px;
}

.url_input {
  width: 300px;
  height: 20px;
  margin: 20px;
}
</style>
