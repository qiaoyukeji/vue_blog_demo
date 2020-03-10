<template>
  <div id="add-blog">
    <h2>添加博客</h2>
    <form v-show="!submited">
      <label for>博客标题</label>
      <input type="text" v-model="blog.title" required />

      <label for>博客内容</label>
      <textarea v-model="blog.content"></textarea>
      <div id="checkboxes">
        <label for>Vue.js</label>
        <input type="checkbox" value="Vue.js" v-model="blog.categories" />

        <label for>Node.js</label>
        <input type="checkbox" value="Node.js" v-model="blog.categories" />

        <label for>React.js</label>
        <input type="checkbox" value="React.js" v-model="blog.categories" />

        <label for>Angular4.js</label>
        <input type="checkbox" value="Angular4.js" v-model="blog.categories" />
      </div>
      <label for>作者：</label>
      <select v-model="blog.author">
        <option v-for="(author,index) in authors" :key="index">{{author}}</option>
      </select>
      <button @click.prevent="post">添加博客</button>
    </form>

    <div v-show="submited">
      <h3>您的博客发布成功</h3>
    </div>

    <div id="preview">
      <h3>博客总览</h3>
      <p>博客标题: {{blog.title}}</p>
      <p>博客内容：</p>
      <p>{{blog.content}}</p>
      <p>博客分类</p>
      <ul>
        <li v-for="(c,index) in blog.categories" :key="index">{{c}}</li>
      </ul>
      <p>作者：{{blog.author}}</p>
    </div>
  </div>
</template>

<script>
// import func from "../../vue-temp/vue-editor-bridge";
export default {
  //http://jsonplaceholder.typicode.com/
  name: "add-blog",
  data() {
    return {
      blog: {
        title: "",
        content: "",
        categories: [],
        author: ""
      },
      authors: ["qiaoyurensheng", "巧遇人生", "qiaoyukeji"],
      submited: false
    };
  },
  methods: {
    post: function() {
      this.submited = true;
      this.$http
        .post("http://jsonplaceholder.typicode.com/posts", {
          title: this.blog.title,
          body: this.blog.content,
          userId: 1
        })
        .then(function(data) {
          console.log(data);
        });
    }
  }
};
</script>

<style scoped>
#add-blog {
  text-align: center;
}
</style>
