<template lang="html">
  <div>
    <article-post
      :done-message="doneMessage"
      :error-message="errorMessage"
      :selected-category-name="selectedCategoryName"
      :category-list="categoryList"
      @selectedCategory="selectedCategory"
    />
  </div>
</template>
<script>
import { ArticlePost } from '@Components/molecules';

export default {
  components: {
    articlePost: ArticlePost,
  },
  computed: {
    doneMessage() {
      return this.$store.state.articles.doneMessage;
    },
    errorMessage() {
      return this.$store.state.articles.errorMessage;
    },
    selectedCategoryName() {
      return this.$store.state.articles.targetArticle.category.name;
    },
    categoryList() {
      return this.$store.state.categories.categoryList;
    },
  },
  created() {
    this.$store.dispatch('categories/getAllCategories');
  },
  methods: {
    selectedCategory($event) {
      const value = $event.target.value;
      console.log(value);
      this.$store.dispatch('articles/selectedArticleCategory', value);
    },
  },
};

</script>
