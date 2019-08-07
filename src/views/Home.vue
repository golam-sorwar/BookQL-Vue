<template>
  <div class="home">
    <div class="container mx-auto px-5 py-5">
      <div class="flex flex-wrap -mx-4">
        <div class="w-full lg:w-1/4 px-4 mb-12">
          <ApolloQuery :query="categoriesQuery">
            <template slot-scope="{ result: { data, loading }, isLoading }">
              <div v-if="isLoading">Loading...</div>
              <ul
                v-else
                class="list-reset p-3 text-lg border border-gray-500 rounded"
              >
                <li class="mb-6">
                  <a
                    href="#"
                    class="text-black hover:text-blue-600"
                    @click.prevent="selectCategory('all')"
                    >All</a
                  >
                </li>
                <li class="mb-6">
                  <a
                    href="#"
                    class="text-black hover:text-blue-600"
                    @click.prevent="selectCategory('featured')"
                    >Featured</a
                  >
                </li>
                <li
                  v-for="category of data.categories"
                  :key="category.id"
                  class="mb-6"
                >
                  <a
                    href="#"
                    class="text-black hover:text-blue-600"
                    @click.prevent="selectCategory(category.id)"
                    >{{ category.name }}</a
                  >
                </li>
                <li class="mb-6">
                  <router-link
                    to="/books/add"
                    class="text-black hover:text-blue-600"
                    >Add a book</router-link
                  >
                </li>
              </ul>
            </template>
          </ApolloQuery>
        </div>
        <div class="w-full lg:w-3/4 px-4 mb-12">
          <div>
            <ApolloQuery :query="query" v-if="selectedCategory === 'all'">
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else class="flex flex-wrap">
                  <div
                    v-for="book of data.books"
                    :key="book.id"
                    class="w-full lg:w-1/3 px-4 mb-12"
                  >
                    <book-listing :book="book"></book-listing>
                  </div>
                </div>
              </template>
            </ApolloQuery>

            <ApolloQuery
              :query="query"
              :variables="{ featured: true }"
              v-else-if="selectedCategory === 'featured'"
            >
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else class="flex flex-wrap">
                  <div
                    v-for="book of data.booksByFeatured"
                    :key="book.id"
                    class="w-full lg:w-1/3 px-4 mb-12"
                  >
                    <book-listing :book="book"></book-listing>
                  </div>
                </div>
              </template>
            </ApolloQuery>

            <ApolloQuery
              :query="query"
              :variables="{ id: selectedCategory }"
              v-else
            >
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else class="flex flex-wrap">
                  <div
                    v-for="book of data.category.books"
                    :key="book.id"
                    class="w-full lg:w-1/3 px-4 mb-12"
                  >
                    <book-listing :book="book"></book-listing>
                  </div>
                </div>
              </template>
            </ApolloQuery>
          </div>
        </div>
      </div>
    </div>
    <!-- end container -->
  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from "../graphql/queries/Category.gql";
import categoriesQuery from "../graphql/queries/Categories.gql";
import booksQuery from "../graphql/queries/Books.gql";
import booksFeaturedQuery from "../graphql/queries/BooksFeatured.gql";
import bookListing from "../components/BookListing.vue";
// eslint-disable-next-line no-unused-vars

export default {
  name: "home",
  components: {
    bookListing
  },
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
      if (category === "all") {
        this.query = booksQuery;
      } else if (category === "featured") {
        this.query = booksFeaturedQuery;
      } else {
        this.query = categoryQuery;
      }
      this.selectedCategory = category;
    }
  }
};
</script>
