<!-- @format -->
<script setup lang="ts">
import { ref, computed } from 'vue'
import type { Ref } from 'vue'

interface UserData {
  user: {
    id: string
    name: string
    profile_image: {
      large: string
    }
    location?: string
  }
}

// Define the searchTerm as a reactive string reference
const searchTerm: Ref<string> = ref('')

// Fetching data from Unsplash API
const { data, status, error } = await useFetch(
  'https://api.unsplash.com/photos?client_id=fFkR35p8U4KfmlxEvH1z_AT6yva4GBiJOI8SSPTZ10c'
)

// Assign data safely by checking if it's available and assigning a fallback value
const userData: Ref<UserData[]> = ref(data?.value || []) as Ref<UserData[]>

// Computed property to filter the user data based on the searchTerm
const filteredData = computed<UserData[]>(() => {
  return userData.value.filter((item) =>
    item?.user?.name.toLowerCase().includes(searchTerm.value.toLowerCase())
  )
})
</script>

<template>
  <div>
    <header class="header">
      <div class="search-bar">
        <div class="search-bar__wrapper">
          <span class="search-bar__icon"></span>
          <input
            type="text"
            placeholder="Search for photo"
            v-model="searchTerm"
          />
        </div>
        <div class="search-results">
          <p v-if="searchTerm">
            Search results for
            <span class="search-results__term">"{{ searchTerm }}"</span>
          </p>
        </div>
      </div>
    </header>
    <main class="main">
      <div v-if="status === 'pending'" class="loading-skeleton">
        <div class="skeleton-card" v-for="index in 3" :key="index"></div>
      </div>
      <div class="content">
        <ContentCard
          v-for="(item, index) in filteredData"
          :key="item.user.id"
          :image="item.user.profile_image.large"
          :author="item.user.name"
          :location="item.user.location || 'No location available'"
        />
      </div>
    </main>
  </div>
</template>

<style>
@import url('~/assets/scss/main.scss');
</style>
