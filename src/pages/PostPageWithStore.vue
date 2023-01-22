<template>
  <div>
    <h1>Posts page with store</h1>
    <router-link to="/">Main</router-link>
    <h1>{{ $store.state.likes }}</h1>
    <my-input
        v-focus
        :model-value="searchQuery"
        @update:model-value="setSearchQuery"
        placeholder="Search..."></my-input>
    <div class="app__btns">
      <my-button
          @click="showDialog"
      >Create
      </my-button>
      <my-select
          :model-value="selectedSort"
          @update:model-value="setSelectedSort"
          :options="sortOptions"
      >
      </my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>
    <div>
      <post-list
          :posts="sortedAndSearchedPosts"
          @remove="removePost"
          v-if="!isPageLoading"
      />
      <div v-else>Page is loading...</div>
      <!--      <my-pagination-->
      <!--          v-model:page="page"-->
      <!--          :total-pages="totalPages"-->
      <!--      >-->
      <!--      </my-pagination>-->
      <div v-intersection="loadMorePosts" class="observer"></div>
    </div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm"
import PostList from "@/components/PostList"
import MyDialog from "@/components/UI/MyDialog";
import MyButton from "@/components/UI/MyButton";
import MySelect from "@/components/UI/MySelect";
import axios from 'axios';
import MyInput from "@/components/UI/MyInput";
import MyPagination from "@/components/UI/MyPagination";
import {mapMutations, mapState, mapActions, mapGetters} from "vuex"

export default {
  components: {
    MyPagination,
    MyInput,
    MyButton,
    MyDialog, MySelect,
    PostList, PostForm
  },
  data() {
    return {
      dialogVisible: false,
    }
  },
  methods: {
    ...mapMutations({
      setPage: 'post/setPage',
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort'
    }),
    ...mapActions({
      loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    }),
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },

  },
  computed: {
    ...mapState({
      posts: state => state.post.posts,
      isPageLoading: state => state.post.isPageLoading,
      selectedSort: state => state.post.selectedSort,
      searchQuery: state => state.post.searchQuery,
      page: state => state.post.page,
      limit: state => state.post.limit,
      totalPages: state => state.post.totalPages,
      sortOptions: state => state.post.sortOptions
    }),
    ...mapGetters({
      sortedPosts: 'post/sortedPosts',
      sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
    })
  },
  mounted() {
    this.fetchPosts()
  }
}
</script>

<style>
.app__btns {
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

.observer {
  height: 30px;
  background: green;
}
</style>

