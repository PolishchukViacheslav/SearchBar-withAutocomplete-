<template>
    <Suspense>
        <div id="show-blogs">
            <h1>All Blog Articles</h1>
            <input type="text" v-model="blogTitle" autocomplete="off" placeholder="search blogs" />
            <!-- <div v-for="blog in filteredBlogs" :key="blog.title" class="single-blog">
                <h2>{{ blog.title }}</h2>
                <article>{{ blog.body }}</article>
            </div> -->
            <div v-if="filteredBlogs">
                <ul>
                    <li v-for="blog in filteredBlogs" :key="blog.id">{{blog.title}}</li>
                </ul>
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
            filteredBlogs: []
        }
    },
    // methods: {
    //     searchBlogTitle(){
    //         this.filteredBlogs = this.blogs.filter(({ title }) => (
    //             title.toLowerCase().indexOf(this.blogTitle.toLowerCase) !== -1
    //         ));
    //         console.log("input",this.blogTitle);
    //     }
    // },
    watch: {
        blogTitle() {
            this.filteredBlogs = this.blogs
                .filter(({ title }) => {
                const regExp = new RegExp(`${this.blogTitle}`,'i');
                return regExp.test(title);
                })
                .map((blog) => ({title: blog.title, id: blog.id}));
            console.log("input",this.filteredBlogs);
            // console.log('blogs', this.filteredBlogs.map(blog => blog.title));
            
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
#show-blogs{
    max-width: 800px;
    margin: 0px auto;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
</style>