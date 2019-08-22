<template>
    <div v-theme:column="'narrow'" id="show-blogs">
        <h1>All Blog Articles</h1>
        <input type="text" v-model="search" placeholder="search blogs" />
        <div v-for="blog in filteredBlogs" class="single-blog" :key="blog.asd">
            <router-link v-bind:to="'/blog/' + blog.id">
                <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
            </router-link>
            <article>{{blog.content | snippet}}</article>
        </div>
    </div>
</template>

<script>

import searchMixin from '../mixins/searchMixin';

export default {

    
    components: {
        
    },
    data() {
        return {
            blogs: [],
            search: ''
        }
    },
    methods: {
        
    },
    //show all datas
    created() {
        //import json **important**
        this.$http.get('https://ninja-vue-playlist.firebaseio.com/posts.json').then(function(data){
            return data.json();
        }).then(function(data){
            var blogsArray = [];
            for (let key in data){
                data[key].id = key
                blogsArray.push(data[key]);
            }
            this.blogs = blogsArray;
        })
    },
    computed: {
        
    },
    //Filters in main.js
    filters: {
        toUppercase(value){
            return value.toUpperCase();
        }
    },
    //Custom Directives in main.js
    directives: {
        'rainbow': {
            bind(el, binding, vnode){
                el.style.color = "#" + Math.random().toString(16).slice(2,8);
            }
        }
    },
    mixins: [
        searchMixin
    ]
}
</script>

<style>
#show-blogs {
    max-width:800px;
    margin:0 auto;
}
.single-blog {
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
</style>