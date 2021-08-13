<template>
  <div id="app">
    <div class="header">
      <div class="imgsection"> 
        <img src="@/assets/caena.svg" alt="caena" class="logo">
      </div>
      <h1 class="title">System Status</h1>
    </div>
    <div class="aws">
      <div v-for="item of items" :key="item.title" class="status_item">
        <h1 class="subheader">{{ item.description }}</h1>
        <p class="text">Last Build Date: {{ item.date }}</p>
        <a class="text" :href="item.link">{{ item.link }}</a>
      </div>
    </div>
  </div>  
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      items: [],
    };
  },
  created() {
    this.getRss("https://status.aws.amazon.com/rss/qldb-us-east-1.rss");
    this.getRss("https://status.aws.amazon.com/rss/sms-us-east-2.rss");
    this.getRss("https://status.aws.amazon.com/rss/detective-ca-central-1.rss");
  },
  methods: {
    async getRss(event) {
      const res = await fetch(
        `https://api.allorigins.win/get?url=${event}`
      );
      const { contents } = await res.json();
      
      const feed = new window.DOMParser().parseFromString(contents, "text/xml");
      const query = feed.querySelectorAll("channel");
      const temp = [...query].map((el) => ({
        description: el.querySelector("description").innerHTML,
        link: el.querySelector("link").innerHTML,
        title: el.querySelector("title").innerHTML,
        date: el.querySelector("lastBuildDate").innerHTML,
      }));
      for (var i of temp) {
        i.description = i.description.replace("<![CDATA[", "").replace("]]>", "");
        this.items.push(i);
      }
      // console.log(contents);
      // console.log(this.items);
    },
  },
};
</script>

<style>
#app {
  /* @import "./scss/main.scss"; */
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500&display=swap');
  font-family: 'Poppins', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #124F4E;
  line-height: 30px; */
}
/* 
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
.url_input {
  width: 400px;
  height: 20px;
  margin: 20px;
} */
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
.subheader {
  font-size: 25px;
  color: #1D817F;
}
.text {
  color: #646363;
  font-size: 15px;
  text-decoration: none;
  margin-top: 25px;
}
.aws {
  color: #646363;
  background-color: #F6F7F9;
  padding: 10px;
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 97%;
  height: 100%;
  /* border-radius: 4px; */
}
.status_item {
  border: 0.5px solid #646363;
  border-radius: 10px;
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 65%;
  margin-bottom: 30px;
  background-color: white;
  padding: 20px;
  
}
</style>