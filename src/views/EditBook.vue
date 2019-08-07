<template>
  <div class="edit container mt-12">
    <h1 class="text-4xl text-center">Edit Book</h1>
    <form action="#" class="w-full max-w-xl mx-64" method="POST" @submit.prevent="editBook">
      <div class="form-group">
        <label class="font-bold mb-2" for="title">Title</label>
        <input type="text" name="title" id="title" v-model="title" class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"/>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="author">Author</label>
        <input type="text" name="author" id="author" v-model="author" class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"/>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="image">Image</label>
        <input type="text" name="image" id="image" v-model="image" class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"/>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="description">Description</label>
        <textarea
          name="description"
          id="description"
          cols="30"
          rows="10"
          v-model="description"
          class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"
        ></textarea>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="link">Link</label>
        <input type="text" name="link" id="link" v-model="link" class="appearance-none block w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500"/>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2"
          ><input
            type="checkbox"
            name="featured"
            v-model="featured"
            class="mr-2"
          />Featured</label
        >
      </div>
      <div class="form-group">
        <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
          <template slot-scope="{ result: { data, loading }, isLoading }">
            <div v-if="isLoading">Loading...</div>
            <select v-else v-model="category_id" class="block appearance-none w-full bg-gray-200 border border-gray-200 text-gray-700 py-3 px-4 pr-8 rounded leading-tight focus:outline-none focus:bg-white focus:border-gray-500">
              <option
                v-for="category of data.categories"
                :key="category.id"
                :value="category.id"
              >
                {{ category.name }}
              </option>
            </select>
          </template>
        </ApolloQuery>
      </div>

      <div class="form-group text-center">
        <button type="submit" class="bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-2 px-4 border-b-4 border-indigo-700 hover:border-indigo-500 rounded">Update book</button>
      </div>
    </form>
  </div>
</template>

<script>
import updateBook from "../graphql/mutations/UpdateBook.gql";
import book from "../graphql/queries/Book.gql";

export default {
  data() {
    return {
      title: "",
      author: "",
      image: "",
      description: "",
      link: "",
      featured: false,
      category_id: 1,
      book: null
    };
  },
  apollo: {
    // Advanced query with parameters
    // The 'variables' method is watched by vue
    book: {
      query: book,
      // Reactive parameters
      variables() {
        if (this.$route && this.$route.params) {
          return {
            id: this.$route.params.id
          };
        }
      },
      // Optional result hook
      result({ data: { book } }) {
        this.title = book.title;
        this.author = book.author;
        this.image = book.image;
        this.description = book.description;
        this.link = book.link;
        this.featured = book.featured;
        this.category_id = book.category.id;
      }
    }
  },
  methods: {
    editBook() {
      this.$apollo
        .mutate({
          // Query
          mutation: updateBook,
          // Parameters
          variables: {
            id: this.$route.params.id,
            title: this.title,
            author: this.author,
            image: this.image,
            link: this.link,
            description: this.description,
            featured: this.featured,
            category_id: this.category_id
          }
        })
        .then(data => {
          console.log(data);
          this.$router.push(`/book/${this.$route.params.id}`);
        })
        .catch(error => {
          console.error(error);
        });
    }
  }
};
</script>

<style scoped>
.form-group {
  margin-bottom: 32px;
}

</style>
