<template>
  <div  v-theme:column="'wide'" id="show-blogs">
    <h1>博客总览</h1>
    <input type="text" v-model="search" placeholder="搜索"/>
    <div class="single-blog" v-for="blog in filterdBlogs">
      <router-link v-bind:to="'/blog/'+blog.id"><h2 v-rainbow>{{blog.title| to-uppercase}}</h2></router-link>
      <article>
        {{blog.body | snippet}}
      </article>
    </div>
  </div>
</template>

<script>
  export default {
    name: "show-blogs",
    data(){
      return{
        blogs:[],
        search:''
      }

    },
    created() {
      this.$http.get("https://jsonplaceholder.typicode.com/posts")
        .then(function (data) {
          this.blogs=data.body.slice(0,10);

      })
    },
    computed:{
      filterdBlogs:function () {
        return this.blogs.filter((blog)=>{
          return blog.title.match(this.search)
        })
      }
    },
    //局部
    filters:{
      "to-uppercase":function (value) {
        return value.toUpperCase();
      }
    },
    directives:{
      'rainbow':{
        bind(el,binding,vnode){
          el.style.color='red';
        }
      }
    }
  }
</script>

<style scoped>
  #show-blogs{
    max-width: 800px;
    margin: 0 auto;
  }
  .single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background-color: #cccccc;
    border: 1px dotted #aaa;
  }
  #show-blogs a{
    text-decoration: none;
    color: #444;
  }
  input[type="text"]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
  }
</style>
