<template lang="html">
  <div>
    <article-post
      :title="title"
      :done-message="doneMessage"
      :error-message="errorMessage"
      :selected-category-name="selectedCategoryName"
      :category-list="categoryList"
      :article-title="articleTitle"
      :article-content="articleContent"
      :markdown-content="markdownContent"
      @selectedCategory="selectedCategory"
      @titleArticle="titleArticle"
      @contentArticle="contentArticle"
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
    };
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
    articleTitle() {
      return this.$store.state.articles.targetArticle.title;
    },
    articleContent() {
      return this.$store.state.articles.targetArticle.content;
    },
    markdownContent() {
      return `# ${this.articleTitle}\n${this.articleContent}`;
    },
  },
  created() {
    this.$store.dispatch('categories/getAllCategories');
  },
  methods: {
    selectedCategory($event) {
      const value = $event.target.value;
      this.$store.dispatch('articles/selectedArticleCategory', value);
    },
    titleArticle($event) {
      const value = $event.target.value;
      this.$store.dispatch('articles/editedTitle', value);
    },
    contentArticle($event) {
      const value = $event.target.value;
      this.$store.dispatch('articles/editedContent', value);
    },
  },
};

</script>
