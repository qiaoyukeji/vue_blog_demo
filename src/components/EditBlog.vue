<template>
  <div id="add-blog">
    <h2 v-if="pass">编辑博客</h2>
    <form v-show="!submited" v-if="pass">
      <label for>博客标题</label>
      <input type="text" v-model="blog.title" required />

      <label for>博客内容</label>
      <textarea v-model="blog.content"></textarea>
      <div id="checkboxes">
        <p>博客分类：</p>
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
      <button @click.prevent="post">编辑博客</button>
    </form>

    <!-- <div>
      <input type="text" placeholder="请输入密码" v-if="!pass" v-model="passwd" @keyup.enter="jiami" />
    </div>-->

    <div v-show="submited">
      <h3>您的博客发布成功</h3>
    </div>

    <div id="preview" v-if="pass">
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
      id: this.$route.params.id,
      blog: {},
      authors: ["qiaoyurensheng", "巧遇人生", "qiaoyukeji"],
      submited: false,
      password: 123456, //设置的密码
      passwd: null, //接受输入的密码
      pass: true //根据密码是否相等，设置显示与隐藏
    };
  },
  methods: {
    post: function() {
      this.submited = true;
      this.$http
        .put("https://my-blog-f3d50.firebaseio.com/posts.json", this.blog)
        .then(function(data) {
          //   console.log(data);
        });
    },
    jiami() {
      // console.log(123);
      if (this.passwd == this.password) {
        this.pass = true;
      } else {
        alert("密码错误，请重新输入！");
      }
    },
    fetchData() {
      //   console.log(this.id);
      this.$http
        .get("https://my-blog-f3d50.firebaseio.com/posts/" + this.id + ".json")
        .then(response => {
          //   console.log(response.body);
          this.blog = response.body;
        });
    }
  },
  created() {
    this.fetchData();
  }
};
</script>

<style scoped>
#add-blog * {
  box-sizing: border-box;
}
#add-blog {
  margin: 20px auto;
  max-width: 600px;
  padding: 20px;
}
label {
  display: block;
  margin: 20px 0 10px;
}
input[type="text"],
textarea,
select {
  display: block;
  width: 100%;
  padding: 8px;
}
textarea {
  height: 200px;
}
#checkboxes label {
  display: inline-block;
  margin-top: 0;
}
#checkboxes input {
  display: inline-block;
  margin-right: 10px;
}
button {
  display: block;
  margin: 20px 0;
  background: crimson;
  color: #fff;
  border: 0;
  padding: 14px;
  border-radius: 4px;
  font-size: 18px;
  cursor: pointer;
}
#preview {
  padding: 10px 20px;
  border: 1px dotted #ccc;
  margin: 30px 0;
}
#preview h3 {
  margin-top: 10px;
}
</style>
