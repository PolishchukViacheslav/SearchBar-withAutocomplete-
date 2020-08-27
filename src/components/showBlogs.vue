<template>
    <Suspense>
        <div class="header">
            <h1>All Blog Articles</h1>
            <div class="header__wrapper">
                <input type="text" v-model="blogTitle" autocomplete="off" placeholder="type blog title here" class="header__input" />
                <!-- <div v-for="blog in filteredBlogs" :key="blog.title" class="single-blog">
                    <h2>{{ blog.title }}</h2>
                    <article>{{ blog.body }}</article>
                </div> -->
                <div v-if="isSearchResult">
                    <ul class="header__autoCompleteList list">
                        <li v-for="blog in filteredBlogs" :key="blog.id" @click="setBlogTitle(blog.title)" class="list__item">
                            <img src="../../public/searchIcon.svg" alt="search icon" class="list__icon">
                            {{blog.title}}
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </Suspense>
</template>

<script>
export default {
    data () {
        return {
            blogs: [],
            blogTitle: '',
            filteredBlogs: [],
            isSearchResult: false
        }
    },
    methods: {
        // searchBlogTitle(){
        //     this.filteredBlogs = this.blogs.filter(({ title }) => (
        //         title.toLowerCase().indexOf(this.blogTitle.toLowerCase) !== -1
        //     ));
        //     console.log("input",this.blogTitle);
        // }
        setBlogTitle(title) {
            this.blogTitle = title;
            this.isSearchResult = false;
            console.log("blogs", this.filteredBlogs, "flag-set",this.isSearchResult);

        }
    },
    watch: {
        blogTitle() {
            console.log('second');
            this.filteredBlogs = this.blogs
                .filter(({ title }) => {
                    if (this.blogTitle) {
                        const regExp = new RegExp(`${this.blogTitle}`,'i');
                        return regExp.test(title)
                    }
                    return false;
                })
                .map((blog) => ({title: blog.title, id: blog.id}));
        }
    },
    
    async mounted() {
        const blogsresponse = await fetch('https://jsonplaceholder.typicode.com/posts');
        this.blogs = await blogsresponse.json();
        console.log(this.blogs);
    },
    // computed: {
    //     filteredBlogs: function(){
    //         return this.blogs.filter((blog) => {
    //             return blog.title.match(this.search);
    //         });
    //     }
    // }
}
</script>

<style>
.header {
    display: grid;
    place-items: center;
    border-bottom: 1px solid #ffa07a;
    width: 100%;
    min-height: 100px;
    margin-bottom: 80px;
    background-color: #f0f8ff;
    font-size: 26px;
}

.header__wrapper {
    position: relative;
    width: 560px;
    padding-bottom: 10px;
}

.header__input {
    padding: 10px;  
    width: inherit;
    border: 1px solid lightgray;
    border-radius: 8px;
}

.header__input:focus {
    outline: none;
    box-shadow: 0 4px 6px 0 rgba(32,33,36,0.28);
}

.header__input::placeholder {
    color: lightgray;
}

.header__autoCompleteList {
    z-index: 1;
    position: absolute;
    margin: 0;
    padding: 0;
    width: inherit;
    background-color:#f0f8ff;
    box-shadow: 0 4px 6px 0 rgba(32,33,36,0.28);
}

.list {
    max-height: 500px;
    overflow: scroll;
    list-style-type: none;
}

.list__item {
    box-sizing: border-box;
    border-bottom: 1px solid #cec7c7;
    display: flex;
    align-items: center;
    padding: 10px;
    width: 560px;
}

.list__item:last-child {
    border: none;
}

.list__icon {
    margin-right: 10px;
    width: 0.7em;
    height: 0.7em;
}

.single-blog {
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
</style>