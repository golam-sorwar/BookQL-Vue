<template>
  <div class="container mx-auto px-8">
    <ApolloQuery
      :query="require('../graphql/queries/Book.gql')"
      :variables="{ id: $route.params.id }"
    >
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else class="flex mt-16 flex-col lg:flex-row">
          <div>
            <img
              :src="`http://localhost:8000/img/${data.book.image}`"
              alt="book cover"
            />
          </div>

          <div class="w-full lg:w-2/3 ml-0 mt-8 lg:mt-0 lg:ml-16">
            <div class="text-4xl font-bold">{{ data.book.title }}</div>
            <div class="text-2xl text-grey-darkest mb-8">
              {{ data.book.author }}
            </div>
            <div class="text-lg text-grey-darkest leading-normal">
              {{ data.book.description }}
            </div>
            <div class="my-12">
              <a
                :href="data.book.link"
                target="_blank"
                class="bg-transparent hover:bg-blue-500 text-blue-700 font-semibold hover:text-white py-2 px-4 border border-blue-500 hover:border-transparent rounded"
                >View Link</a
              >
            </div>
            <router-link :to="`/books/${data.book.id}/edit`" href="#" class="bg-transparent hover:bg-yellow-500 text-black-500 font-semibold hover:text-white py-2 px-4 border border-yellow-600 hover:border-transparent rounded"
              >Edit</router-link
            >
            &middot;
            <a href="#" class="bg-transparent hover:bg-red-700 text-red-700 font-semibold hover:text-white py-2 px-4 border border-red-500 hover:border-transparent rounded" @click.prevent="deleteBook">Delete</a>
          </div>
          <div></div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import deleteBook from "../graphql/mutations/DeleteBook.gql";
export default {
  methods: {
    deleteBook() {
      this.$apollo
        .mutate({
          mutation: deleteBook,
          variables: {
            id: this.$route.params.id
          }
        })
        .then(data => {
          console.log(data);
          this.$router.push("/");
        });
    }
  }
};
</script>
