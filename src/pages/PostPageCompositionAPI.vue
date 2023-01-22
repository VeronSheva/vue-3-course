<template>
  <div>
    <h1>Posts page</h1>
    <router-link to="/">Main</router-link>
    <my-input
        v-focus
        v-model="searchQuery"
        placeholder="Search..."></my-input>
    <div class="app__btns">
      <my-button
      >Create
      </my-button>
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      >
      </my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form
      />
    </my-dialog>
    <div>
      <post-list
          :posts="sortedAndSearchedPosts"
          v-if="!isPageLoading"
      />
      <div v-else>Page is loading...</div>
      <!--      <my-pagination-->
      <!--          v-model:page="page"-->
      <!--          :total-pages="totalPages"-->
      <!--      >-->
      <!--      </my-pagination>-->
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
import {usePosts} from "@/hooks/usePosts";
import useSortedandSearchedPosts from "@/hooks/useSortedandSearchedPosts";
import useSortedPosts from "@/hooks/useSortedPosts";

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
      sortOptions: [
        {value: 'title', name: 'By name'},
        {value: 'body', name: 'By content'}
      ]
    }
  },
  setup(props) {
    const {posts, isPageLoading, totalPages} = usePosts(10)
    const {sortedPosts, selectedSort} = useSortedPosts(posts)
    const {searchQuery, sortedAndSearchedPosts} = useSortedandSearchedPosts(sortedPosts)

  return {
    posts,
    isPageLoading,
    totalPages,
    sortedAndSearchedPosts,
    sortedPosts,
    searchQuery,
    selectedSort
  }
  },
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

