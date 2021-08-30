<template lang="html">
  <div class="article-post">
    <div v-if="doneMessage">
      <app-text bg-success>{{ doneMessage }}</app-text>
    </div>
    <div>
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
        />
        <div v-if="errorMessages">
          <app-text bg-error>{{ errorMessage }}</app-text>
        </div>
        <app-textarea
          placeholder="記事の本文をマークダウン記法で入力してください。"
        />
        <app-button
          round
        >
          作成
        </app-button>
        <article>
          <app-markdown-preview />
        </article>
      </section>
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
  },
};
</script>

<style lang="postcss" scoped>
.article-post {
  /* display: flex; */
}

</style>