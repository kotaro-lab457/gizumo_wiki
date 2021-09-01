<template lang="html">
  <div>
    <article-post
      :error-message="errorMessage"
      :selected-category-name="selectedCategoryName"
      :category-list="categoryList"
      :article-title="articleTitle"
      :article-content="articleContent"
      :markdown-content="markdownContent"
      :loading="loading"
      :access="access"
      @selectedCategory="selectedCategory"
      @titleArticle="titleArticle"
      @contentArticle="contentArticle"
      @handleSubmit="handleSubmit"
    />
  </div>
</template>
<script>
import { ArticlePost } from '@Components/molecules';

export default {
  components: {
    articlePost: ArticlePost,
  },
  data() {
    return {
      title: '',
      content: '',
    };
  },
  computed: {
    errorMessage() {
      return this.$store.state.articles.errorMessage;
    },
    selectedCategoryName() {
      return this.$store.state.articles.targetArticle.category.name;
    },
    categoryList() {
      return this.$store.state.categories.categoryList;
    },
    articleTitle() {
      return this.$store.state.articles.targetArticle.title;
    },
    articleContent() {
      return this.$store.state.articles.targetArticle.content;
    },
    markdownContent() {
      return `# ${this.articleTitle}\n${this.articleContent}`;
    },
    loading() {
      return this.$store.state.articles.loading;
    },
    access() {
      return this.$store.getters['auth/access'];
    },
  },
  created() {
    this.$store.dispatch('categories/getAllCategories');
    this.$store.dispatch('articles/initPostArticle');
  },
  methods: {
    selectedCategory($event) {
      const categoryName = $event.target.value;
      this.$store.dispatch('articles/selectedArticleCategory', categoryName);
    },
    titleArticle($event) {
      const title = $event.target.value;
      this.$store.dispatch('articles/editedTitle', title);
    },
    contentArticle($event) {
      const content = $event.target.value;
      this.$store.dispatch('articles/editedContent', content);
    },
    handleSubmit() {
      if (this.loading) return;
      this.$store.dispatch('articles/postArticle').then(() => {
        this.$router.push({
          path: '/articles',
          query: { redirect: '/article/post' },
        });
      });
    },
  },
};

</script>
