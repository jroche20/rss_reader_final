<template>
  <div id="app">
    <div class="header">
      <div class="imgsection"> 
        <img src="@/assets/caena.svg" alt="caena" class="logo">
      </div>
      <h1 class="title">System Status</h1>
    </div>
    <form @submit.prevent="getRss">
      <div>
        <label> rss url</label>
        <br />
        <input v-model="rssUrl" class="url_input" placeholder="url"/>
      </div>
      <input type="submit" class="submit"/>
    </form>
    <div v-for="item of items" :key="item.title">
      <h1 class="text">Title: {{ item.title }}</h1>
      <p class="text">Last Build Date: {{ item.date }}</p>
      <a class="link" :href="item.link">{{ item.link }}</a>
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
      const urlRegex = /(http(s)?:\/\/.)?(www\.)?[-a-zA-Z0-9@:%._+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_+.~#?&//=]*)/g;
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
      const items = feed.querySelectorAll("channel");
      console.log(feed);
      this.items = [...items].map((el) => ({
        link: el.querySelector("link").innerHTML,
        title: el.querySelector("title").innerHTML,
        date: el.querySelector("lastBuildDate").innerHTML,
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
  color: #124F4E;
  line-height: 30px;
}

.submit {
  font-family: 'Poppins', sans-serif;
  background-color: #1D817F;
  text-decoration: none;
  color: white;
  width: 100px;
  padding: 5px;
  font-size: 16px;
  border-radius: 4px;
}

form {
  background-color: rgba(246, 247, 249, 0.5);
  padding: 20px;
  margin: 40px;
}

.url_input {
  width: 400px;
  height: 20px;
  margin: 20px;
}

.header {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;
}

.logo {
    width: 150px;
    height: 50px;
    padding: 9px;
    /* margin-left: 20px; */
    /* margin-bottom: 30px;  */
}

.title {
    padding: 20px;
    background: #1D817F;
    color: white;
    font-size: 20px;
    width: 100%;
}

.imgsection {
  width: 200px;
  display: flex;
  align-items: center;
}

.text {
  color: #124F4E;
  font-size: 25px;
}

.link {
  text-decoration: none;
  color: #124F4E;
}
</style>
