<template>
  <div>
    <ApolloQuery
      :query="require('../graphql/queries/Book.gql')"
      :variables="{ id: $route.params.id }"
    >
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div>
            <h3>{{ data.book.title }}</h3>
          </div>
          <div>{{ data.book.author }}</div>
          <img
            :src="`http://localhost:8000/img/${data.book.image}`"
            alt="cover image"
          />
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>
