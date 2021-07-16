<template>
  <input
    v-model="query"
    class="search"
    type="text"
    placeholder="Search" />
  <div class="results">
    <template v-if="loading">
      Loading...
    </template>
    <template v-else-if="result">
      <div
        v-for="character in result.characters.results"
        :key="character.id"
        class="result">
        <CharacterCard
          :character="character"
          @click="emit('character', character.id)" />
      </div>
      <div class="buttons">
        <div>
          <Button
            v-if="result.characters.info.prev"
            @click="page--">
            Previous
          </Button>
        </div>
        <div>
          <Button
            v-if="result.characters.info.next"
            @click="page++">
            Next
          </Button>
        </div>
      </div>
    </template>
  </div>
</template>

<script setup>
import { ref, defineEmits, watch } from 'vue'
import { useQuery } from '@vue/apollo-composable'
import gql from 'graphql-tag'
import CharacterCard from './CharacterCard.vue'
import Button from './Button.vue'

const emit = defineEmits(['character'])

const query = ref('')
const page = ref(1)

watch(page, () => window.scrollTo(0, 0))

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
.search {
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
  cursor: pointer;

  & + & {
    margin-top: 16px;
  }
}

.buttons {
  margin-top: 24px;
  display: flex;
  justify-content: space-between;
}
</style>
