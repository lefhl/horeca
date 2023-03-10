<template>
  <div class="page">
    <div class="wrapper">
      <template v-if="data.success">
        <guide-the-search
          :title="pageInfo.name"
          :description="pageInfo.description"
          class="page__search"
        />
        <template v-for="(item, index) in pageInfo.content" :key="index">
          <article
            v-if="item.type === 'block'"
            :id="item.id || null"
            class="term page__section"
          >
            <div class="term__inner">
              <template
                v-for="(content, contentIndex) in item.content"
                :key="contentIndex"
              >
                <h2 v-if="content.type === 'title'" class="term__title">
                  {{ content.content }}
                </h2>
                <p
                  v-else-if="content.type === 'text'"
                  class="term__text"
                  v-html="content.content"
                ></p>
                <h4 v-if="content.type === 'subtitle'" class="term__subtitle">
                  {{ content.content }}
                </h4>
                <ol v-if="content.type === 'list'" class="term__list">
                  <li
                    v-for="(listItem, listIdx) in content.content"
                    :key="listIdx"
                    v-html="listItem"
                  ></li>
                </ol>
              </template>
            </div>
          </article>
          <figure v-else>
            <img
              :src="item.content"
              :alt="item.imageTitle"
              class="page__section"
            />
            <figcaption
              v-if="item.caption"
              class="term__pic-caption"
              v-html="item.caption"
            ></figcaption>
          </figure>
        </template>
      </template>
      <h1 v-else>Не найдено</h1>
      <div class="common-links">
        <the-link :to="{ name: 'guide' }">
          Главная веганского справочника
        </the-link>
        <the-link :to="{ name: 'suppliers' }">
          Посмотреть поставщиков
        </the-link>
      </div>
    </div>
  </div>
</template>
<script setup>
useHead({
  titleTemplate: 'HoReCa - о нас',
});

const route = useRoute();
const { data } = await useAsyncData('article', () =>
  $fetch('/api/article', { query: { id: route.params.id } }),
);

const pageInfo = computed(() => data.value.response);
</script>

<style lang="scss" scoped>
.page {
  &__section {
    margin-top: 98px;
  }

  @include media('sm') {
    &__section {
      margin-top: 70px;
    }
  }

  @include media('xs') {
    &__section {
      margin-top: 36px;
    }
  }
}
.term {
  --radius: 40px;
  padding: 100px;
  border: 1.5px solid #000000;
  &:first-of-type {
    border-radius: var(--radius) var(--radius) 0px 0px;
  }
  &:last-of-type:not(:first-of-type) {
    border-radius: 0px 0px var(--radius) var(--radius);
  }

  &__inner {
    max-width: 952px;
  }

  &__title {
    margin-bottom: 58px;
  }

  &__text {
    line-height: 1.42;
  }

  &::v-deep(a) {
    color: var(--title-color);
    text-decoration: underline;
    text-decoration-skip-ink: none;
    &:hover {
      text-decoration: none;
    }
  }

  &__subtitle {
    margin-top: 78px;
    margin-bottom: 22px;
  }

  &__list {
    margin-top: 38px;
    margin-left: 1rem;
    line-height: 1.73;
  }

  &__pic-caption {
    font-weight: 500;
    font-size: 26px;
    line-height: 35px;
    color: #aea9a9;
    margin-top: 40px;
  }

  @include media('sm') {
    padding: 60px;

    &__title {
      margin-bottom: 30px;
    }

    &__subtitle,
    &__list {
      margin-top: 30px;
    }

    &__pic-caption {
      font-size: 20px;
      margin-top: 20px;
      line-height: 1;
    }
  }

  @include media('xs') {
    --radius: 20px;
    padding: 30px 20px;

    &__list,
    &__text {
      line-height: 1.125;
    }

    &__title {
      font-size: 30px;
      line-height: 1;
      margin-bottom: 20px;
    }

    &__subtitle,
    &__list {
      margin-top: 20px;
    }

    &__subtitle {
      letter-spacing: 0.035em;
      line-height: 1;
      font-size: 18px;
    }

    &__pic-caption {
      font-size: 12px;
      margin-top: 8px;
    }
  }
}
</style>
