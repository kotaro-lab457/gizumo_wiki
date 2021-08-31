<template lang="html">
  <div>
    <div v-if="doneMessage">
      <app-text bg-success>{{ doneMessage }}</app-text>
    </div>
    <div class="article-post">
      <section>
        <app-heading :level="1">記事の新規作成</app-heading>
        <app-heading :level="2">カテゴリーの選択</app-heading>
        <app-select
          v-validate="'required'"
          name="category"
          data-vv-as="カテゴリー"
          :value="selectedCategoryName"
          :error-messages="errors.collect('category')"
          @updateValue="$emit('selectedCategory', $event)"
        >
          <option
            value=""
            selected
            disabled
          >
            ---
          </option>
          <option
            v-for="category in categoryList"
            :key="category.id"
            :value="category.name"
          >
            {{ category.name }}
          </option>
        </app-select>
        <div v-if="errorMessages">
          <app-text bg-error>{{ errorMessage }}</app-text>
        </div>
        <app-heading :level="2">タイトル・本文</app-heading>
        <app-input
          v-validate="'required'"
          name="title"
          type="text"
          placeholder="記事のタイトルを入力してください。"
          data-vv-as="タイトル"
          :error-messages="errors.collect('title')"
          :value="articleTitle"
          @updateValue="$emit('titleArticle', $event)"
        />
        <div v-if="errorMessages">
          <app-text bg-error>{{ errorMessage }}</app-text>
        </div>
        <app-textarea
          v-validate="'required'"
          name="content"
          placeholder="記事の本文をマークダウン記法で入力してください。"
          data-vv-as="記事の本文"
          :error-messages="errors.collect('content')"
          :value="articleContent"
          @updateValue="$emit('contentArticle', $event)"
        />
        <app-button
          round
          type="button"
          @click="handleSubmit"
        >
          作成
        </app-button>
      </section>
        <article>
          <app-markdown-preview
            :markdown-content="markdownContent"
          />
        </article>
    </div>
  </div>
</template>

<script>
import {
  Heading,
  Input,
  Textarea,
  MarkdownPreview,
  Button,
  Select,
  Text,
} from '@Components/atoms';

export default {
  components: {
    appHeading: Heading,
    appInput: Input,
    appTextarea: Textarea,
    appMarkdownPreview: MarkdownPreview,
    appButton: Button,
    appSelect: Select,
    appText: Text,
  },
  props: {
    title: {
      type: String,
      default: '',
    },
    doneMessage: {
      type: String,
      default: '',
    },
    errorMessage: {
      type: String,
      default: '',
    },
    selectedCategoryName: {
      type: String,
      default: '',
    },
    categoryList: {
      type: Array,
      default: () => [],
    },
    articleTitle: {
      type: String,
      default: '',
    },
    articleContent: {
      type: String,
      default: '',
    },
    markdownContent: {
      type: String,
      default: '',
    },
    access: {
      type: Object,
      default: () => ({}),
    }
  },
  methods: {
    handleSubmit() {
      if(!this.access.create) return;
      this.$emit('clearMessage');
      this.$validator.validate().then((valid) => {
        if (valid) this.$emit('handleSubmit');
      });
    }
  },
};
</script>

<style lang="postcss" scoped>
.article-post {
  display: flex;
}

</style>