<template>
  <div class="search">
    <input
      v-model="query"
      type="text"
      placeholder="Search" />
    <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path fill-rule="evenodd" clip-rule="evenodd" d="M10.0589 6.77944C10.0589 8.59063 8.59063 10.0589 6.77944 10.0589C4.96826 10.0589 3.5 8.59063 3.5 6.77944C3.5 4.96826 4.96826 3.5 6.77944 3.5C8.59063 3.5 10.0589 4.96826 10.0589 6.77944ZM9.58708 10.6477C8.79881 11.2208 7.8286 11.5589 6.77944 11.5589C4.13983 11.5589 2 9.41905 2 6.77944C2 4.13983 4.13983 2 6.77944 2C9.41905 2 11.5589 4.13983 11.5589 6.77944C11.5589 7.82859 11.2208 8.79878 10.6477 9.58704L14.001 12.9403L12.9403 14.001L9.58708 10.6477Z" fill="#161616"/>
    </svg>
  </div>
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
import { ref, watch } from 'vue'
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
  position: relative;

  input {
    border: 1px solid #D7DFE9;
    border-radius: 8px;
    padding: 12px 16px 12px 48px;
    width: 100%;

    &::placeholder {
      color: #2E343B;
    }
  }

  svg {
    position: absolute;
    left: 16px;
    top: 16px;
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
