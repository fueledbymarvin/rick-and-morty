<template>
  <div class="title">Character Explorer</div>
  <input
    v-model="query"
    class="search"
    type="text"
    placeholder="Search" />
  <div v-if="query" class="results">
    <div v-if="loading">
      Loading...
    </div>
    <div v-else-if="result">
      <div
        v-for="c in result.characters.results"
        :key="c.id"
        class="result">
        <img :src="c.image" class="result-image" />
        <div class="result-info">
          <div class="result-name truncate">{{ c.name }}</div>
          <div class="result-location truncate">{{ c.location.name }}</div>
        </div>
      </div>
      <div class="buttons">
        <div>
          <button
            v-if="result.characters.info.prev"
            @click="page--">
            Previous
          </button>
        </div>
        <div>
          <button
            v-if="result.characters.info.next"
            @click="page++">
            Next
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useQuery } from '@vue/apollo-composable'
import gql from 'graphql-tag'

const query = ref('')
const page = ref(1)

const { result, loading } = useQuery(
  gql`
    query Search($query: String!, $page: Int!) {
      characters(page: $page, filter: { name: $query }) {
        info {
          count
          pages
          next
          prev
        }
        results {
          id
          name
          image
          location {
            id
            name
          }
        }
      }
    }
  `,
  { query, page },
  { debounce: 200 },
)
</script>

<style lang="scss" scoped>
.title {
  font-weight: bold;
  font-size: 24px;
}

.search {
  margin-top: 16px;
  border: 1px solid #D7DFE9;
  border-radius: 8px;
  padding: 12px 16px;
  width: 100%;

  &::placeholder {
    color: #2E343B;
  }
}

.results {
  margin-top: 24px;
}

.result {
  display: flex;
  border-radius: 8px;
  overflow: hidden;
  box-shadow:
    0px 2px 4px rgba(58, 92, 144, 0.14),
    0px 3px 4px rgba(58, 92, 144, 0.12),
    0px 1px 5px rgba(58, 92, 144, 0.2);

  & + & {
    margin-top: 16px;
  }
}

.result-image {
  flex-shrink: 0;
  width: 96px;
  height: 96px;
}

.result-info {
  min-width: 0;
  padding: 16px;
}

.result-name {
  font-size: 20px;
  font-weight: bold;
}

.result-location {
  color: #878D96;
  margin-top: 8px;
}

.truncate {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.buttons {
  margin-top: 24px;
  display: flex;
  justify-content: space-between;

  button {
    background: #FFF;
    border: 1px solid #343A3F;
    box-shadow:
      0px 2px 4px rgba(58, 92, 144, 0.14),
      0px 3px 4px rgba(58, 92, 144, 0.12),
      0px 1px 5px rgba(58, 92, 144, 0.2);
    border-radius: 8px;
    padding: 8px 16px;
    text-transform: uppercase;
    color: #343A3F;
    font-size: 12px;
    font-weight: bold;
    line-height: 1;
  }
}
</style>
