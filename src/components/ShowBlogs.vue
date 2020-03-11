<template>
  <div v-theme:column="''" id="show-blogs">
    <h1>博客总览</h1>
    <input type="text" placeholder="搜索" v-model="search" />
    <div v-for="(blog,index) in filteredBlogs" class="single-blog" :key="index">
      <router-link v-bind:to="'/blog/'+blog.id">
        <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
      </router-link>
      <p class="right">{{blog.time}}</p>
      <p>{{blog.content | snippet}}</p>
    </div>
  </div>
</template>

<script>
export default {
  //https://jsonplaceholder.typicode.com/posts
  name: "show-blogs",
  data() {
    return {
      blogs: [],
      search: ""
    };
  },
  created() {
    this.$http
      .get("https://my-blog-f3d50.firebaseio.com/posts.json")
      .then(function(data) {
        // console.log(data.json());
        return data.json();
        //只获取body中的10个对象
        // this.blogs = data.body.slice(0, 10);
        // console.log(this.blogs);
      })
      .then(function(data) {
        var blogsArray = [];
        for (let key in data) {
          // console.log(data[key]);
          //对象中新建一个id
          data[key].id = key;
          blogsArray.push(data[key]);
        }
        this.blogs = blogsArray;
      });
  },
  //搜索功能
  computed: {
    filteredBlogs: function() {
      return this.blogs.filter(blog => {
        return blog.title.match(this.search);
      });
    }
  }
};
</script>

<style scoped>
#show-blogs {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background: #bbb;
}
.single-blog {
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eee;
  border: 1px dotted #aaa;
}
#show-blogs a {
  color: #444;
  text-decoration: none;
}
input {
  padding: 8px;
  width: 100%;
  box-sizing: border-box;
}
.right {
  float: right;
  box-sizing: border-box;
}
</style>
