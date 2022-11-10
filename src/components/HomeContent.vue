<script>
export default {
  props: {
    meta: {
      type: Object,
      required: true,
    },
    currentPage: {
      type: [String, Number],
      default: 1,
    },
    animes: {
      type: Array,
      required: true,
    },
  },
  data: () => ({
    q: '',
  }),
  computed: {
    totalPages() {
      const lastPage = Math.ceil(this.meta.total_count / 12);
      return lastPage;
    },
    prevPageDisabled() {
      return Number(this.currentPage) <= 1;
    },
    nextPageDisabled() {
      return Number(this.currentPage) >= this.totalPages;
    },
    prevUrl() {
      if (this.prevPageDisabled) {
        return '#';
      }

      return `/page/${Number(this.currentPage) - 1}`;
    },
    nextUrl() {
      if (this.nextPageDisabled) {
        return '#';
      }

      return `/page/${Number(this.currentPage) + 1}`;
    },
  },
  methods: {
    search() {
      window.location.href = `/?s=${this.q}`;
    },
  }
}
</script>

<template>
  <div class="section has-background-primary">
    <form class="box" @submit.prevent="search">
      <div class="field">
        <p class="control">
          <label for="search">
            Rechercher
          </label>
          <input v-model="q" type="search" class="input">
        </p>
      </div>
    </form>

    <div class="columns is-multiline">
      <article
        v-for="anime in animes"
        :key="anime.id"
        class="column is-4"
      >
        <div class="box" style="height: 100%">
          <h2 class="title">
            {{ anime.title }}
          </h2>
          <p class="mt-2">{{ anime.published_at }}</p>
          <div class="mt-2">
            <a :href="`/anime/${anime.id}`">
              Voir cette bande annonce
            </a>
          </div>  
        </div>
      </article>
    </div>

    <div class="box level">
      <div class="level-left">
        <nav class="buttons">
          <a
            v-for="n in totalPages"
            :key="n"
            class="button is-info"
            :disabled="currentPage == n"
            :href="`/page/${n}`"
          >
            {{ n }}
          </a>
        </nav>
      </div>

      <div class="level-right">
        <nav class="buttons">
          <a
            class="button is-info"
            type="button"
            :disabled="prevPageDisabled"
            :href="prevUrl"
          >
            Page précédente
          </a>
          <a
            class="button is-info"
            type="button"
            :disabled="nextPageDisabled"
            :href="nextUrl"
          >
            Page suivante
          </a>
        </nav>
      </div>
    </div>
  </div>
</template>