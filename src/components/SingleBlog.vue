<template>
  <div id="single-blog">
    <div class="content">
      <h1>标题：{{blog.title}}</h1>
      <hr />
      <p>正文内容：</p>
      <article>{{blog.content}}</article>
    </div>
    <p>作者：{{blog.author}}</p>
    <p>分类：</p>
    <ul>
      <li v-for="c in blog.categories">{{c}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "single-blog",
  data() {
    return {
      id: this.$route.params.id,
      blog: {}
    };
  },
  created() {
    this.$http
      .get("https://my-blog-f3d50.firebaseio.com/posts/" + this.id + ".json")
      .then(function(data) {
        // console.log(data);
        // this.blog = data.body;
        return data.json();
      })
      .then(function(data) {
        this.blog = data;
      });
  }
};
</script>

<style scoped>
#single-blog {
  max-width: 960px;
  margin: 0 auto;
  padding: 20px;
  background: #eee;
  border: 1px dotted #aaa;
}
.content {
  height: 600px;
  border: 1px dotted #aaa;
  padding: 10px 15px;
  margin: 5px;
  box-sizing: border-box;
}
</style> 