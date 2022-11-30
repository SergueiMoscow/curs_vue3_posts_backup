<template>
    <div class="app">
      <h1>Страница с постами</h1>
      <!--input type="text" v-model.number="modificatorValue" /-->
      <!-- <my-button @click="fetchPosts">Получить посты</my-button> -->
      <my-input 
        v-model="searchQuery"
        placeholder="Поиск..."
      />
      <div class="app__btns">
        <my-button @click="showDialog">Создать пост</my-button>
        <my-select  v-model="selectedSort" :options="sortOptions"></my-select>
      </div>
      <my-dialog v-model:show="dialogVisible">
        <post-form  @create="createPost"/>
      </my-dialog>
      <post-list 
        :posts="searchedAndSortedPosts" 
        @remove="removePost" 
        v-if="!isPostsLoading"/>
      <div v-else>Идёт загрузка</div>
      <div ref="observer" class="observer"></div>
    </div>
    </template>
    <script>
    import PostForm from "@/components/PostForm";
    import PostList from "@/components/PostList";
    import axios from 'axios';
    
    export default {
      components: {
        PostList, PostForm
      },
      data() {
        return {
          posts: [],
          dialogVisible: false,
          modificatorValue: '',
          isPostsLoading: false,
          selectedSort: '',
          searchQuery: '',
          page: 1,
          limit: 10,
          totalPages: 0,
          sortOptions: [
            {value: 'title', name: 'По названию'},
            {value: 'body', name: 'По содержимому'},
          ],
        }
      },
      methods: {
        createPost(post) {
          this.posts.push(post);
          this.dialogVisible = false;
        },
        removePost(post) {
          this.posts = this.posts.filter(p => p.id !== post.id);
        },
        showDialog() {
          this.dialogVisible = true
        },
        async fetchPosts() {
          try {
            this.isPostsLoading = true
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
              params: {
                _page: this.page,
                _limit: this.limit
              }
            });
            this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
            this.posts = response.data;
          } catch (e) {
            alert('Ошибка связи с сервером')
          } finally {
            this.isPostsLoading = false
          }
        },
        async loadMorePosts() {
          try {
            this.page += 1
            //this.isPostsLoading = true
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
              params: {
                _page: this.page,
                _limit: this.limit
              }
            });
            this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
            this.posts = [...this.posts, ...response.data];
          } catch (e) {
            alert('Ошибка связи с сервером')
          } finally {
            //this.isPostsLoading = false
          }
        },
    
      },
      mounted() {
        this.fetchPosts();
        console.log(this.$refs.observer)
        const options = {
          //root: document.querySelector('#scrollArea'),
          rootMargin: '0px',
          threshold: 1.0
        }
        const callback =  (entries, observer) => {
          /* Content excerpted, show below */
          if (entries[0].isIntersecting && this.page < this.totalPages) {
              //console.log(entries)
              this.loadMorePosts()
            }    
          };
        const observer = new IntersectionObserver(callback, options);  
        observer.observe(this.$refs.observer)  
      },
      computed: {
        sortedPosts() {
          return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
          )
        },
        searchedAndSortedPosts() {
          return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
      },
      watch: {
      }
    }
    </script>
    <style>
    .page__wrapper {
      display: flex;
      margin-top: 15px;
    }
    .page {
      border: 1px solid black;
      padding: 10px;
      margin: 5px;
      cursor: pointer;
    }
    .current-page {
      border: 3px solid teal;
    }
    .observer {
      height: 30px;
      background: gray;
    }
    </style>
    