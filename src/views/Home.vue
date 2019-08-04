<template>
  <div class="home">
    <ApolloQuery :query="categoriesQuery">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <a
            href="#"
            v-for="category of data.categories"
            :key="category.id"
            @click="selectCategory(category.id)"
            class="link-margin"
            >{{ category.id }} {{ category.name }}</a
          >
        </div>
      </template>
    </ApolloQuery>
    <div v-if="selectedCategory == 'all'">
      <ApolloQuery :query="query">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.books" :key="book.id"></div>
          </div>
        </template>
      </ApolloQuery>
    </div>
    <div v-else-if="selectedCategory == 'featured'">
      <ApolloQuery :query="query">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.books" :key="book.id"></div>
          </div>
        </template>
      </ApolloQuery>
    </div>
    <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.category.books" :key="book.id">
            {{ book.id }} {{ book.title }}
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from "../graphql/queries/Category.gql";
import categoriesQuery from "../graphql/queries/Categories.gql";
import booksQuery from "../graphql/queries/Books.gql";
import booksFeaturedQuery from "../graphql/queries/BooksFeatured.gql";
// eslint-disable-next-line no-unused-vars

export default {
  name: "home",
  components: {},
  data() {
    return {
      categoryQuery,
      booksQuery,
      categoriesQuery,
      booksFeaturedQuery,
      selectedCategory: "all",
      query: booksQuery,
      categories: []
    };
  },
  methods: {
    selectCategory(category) {
      if (category == "all") {
        this.query = booksQuery;
      } else if (category == "featured") {
        this.query = booksFeaturedQuery;
      } else {
        this.query = categoryQuery;
      }
      this.selectedCategory = category;
    }
  }
};
</script>

<style>
.link-margin {
  margin-left: 24px;
}
</style>
