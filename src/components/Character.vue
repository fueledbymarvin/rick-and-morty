<template>
  <template v-if="loading">
    Loading...
  </template>
  <template v-else>
    <div class="profile">
      <img :src="result.character.image" />
      <div class="info">
        <div class="name">
          {{ result.character.name }}
        </div>
        <div class="details">
          {{ result.character.location.name }}<br />
          {{ result.character.species }}<br />
          {{ result.character.gender }}<br />
          {{ result.character.status }}<br />
          {{ result.character.episode.length }}<br />
        </div>
      </div>
    </div>

    <div class="title">
      Seen with
    </div>
    <template v-for="(character, index) in seenWith">
      <div
        class="card"
        v-if="index < 5"
        :key="character.id"
        @click="emit('character', character.id)">
        <CharacterCard :character="character" />
      </div>
    </template>
    <div class="title">
      Near
    </div>
    <template v-for="(character, index) in near">
      <div
        class="card"
        v-if="index < 5"
        :key="character.id"
        @click="emit('character', character.id)">
        <CharacterCard :character="character" />
      </div>
    </template>
  </template>
</template>

<script setup>
import { computed, toRefs, watch } from 'vue'
import { useQuery } from '@vue/apollo-composable'
import gql from 'graphql-tag'
import CharacterCard from './CharacterCard.vue'

const emit = defineEmits(['character'])

const props = defineProps({
  id: String,
})
const { id } = toRefs(props)

watch(id, () => window.scrollTo(0, 0))

const { result, loading } = useQuery(
  gql`
    query Character($id: ID!) {
      character(id: $id) {
        id
        name
        image
        location {
          id
          name
          residents {
            id
            name
            image
            location {
              id
              name
            }
          }
        }
        species
        gender
        status
        episode {
          id
          characters {
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
    }
  `,
  { id },
)

const seenWith = computed(() => result.value?.character?.episode?.flatMap((e) => e.characters))
const near = computed(() => result.value?.character?.location?.residents)
</script>

<style lang="scss" scoped>
.profile {
  border-radius: 8px;
  overflow: hidden;
  box-shadow:
    0px 2px 4px rgba(58, 92, 144, 0.14),
    0px 3px 4px rgba(58, 92, 144, 0.12),
    0px 1px 5px rgba(58, 92, 144, 0.2);

  img {
    width: 100%;
  }
}

.info {
  padding: 16px;
}

.name {
  font-size: 20px;
  font-weight: bold;
}

.details {
  margin-top: 8px;
  color: #878D96;
}

.title {
  margin-top: 32px;
  font-size: 24px;
  font-weight: bold;
}

.card {
  margin-top: 16px;
  cursor: pointer;
}
</style>
