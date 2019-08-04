<template>
  <div class="home">
    <ApolloQuery :query="require('../graphql/queries/Categories.gql')">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <ul v-else>
          <a
            href="#"
            v-for="category of data.categories"
            :key="category.id"
            class="user"
          >
            {{ category.id }} {{ category.name }}
          </a>
        </ul>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src
import gql from "graphql-tag";
// eslint-disable-next-line no-unused-vars

export default {
  name: "home",
  components: {},
  data() {
    return {
      categories: []
    };
  },
  apollo: {
    // Simple query that will update the 'hello' vue property
    categories: gql`
      {
        categories {
          id
          name
        }
      }
    `
  }
};
</script>

<style>
.link-margin {
  margin-left: 24px;
}
</style>
