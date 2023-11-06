<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title class="font">
          Berita.Com
        </q-toolbar-title>

        <q-btn
          flat
          dense
          round
          icon="search"
          aria-label="Cari"
          @click="toggleSearchBar"
        />

        <q-input
          dense
          clearable
          v-model="searchTerm"
          placeholder="Cari Berita..."
          @blur="toggleSearchBar"
          @keyup.enter="performSearch"
        />
      </q-toolbar>

      <div class="q-pa-md q-gutter-md row items-center justify-around">
        <a
          v-for="category in categories"
          :key="category.value"
          dense
          @click="selectCategory(category.value)"
          :class="[
            'q-mx-sm',
            'cursor-pointer',
            selectedCategory === category.value ? 'font-weight-bold text-negative' : 'bold-text'
          ]"
        >
          {{ category.label }}
        </a>
      </div>
    </q-header>

    <q-page-container>
      <div class="q-pa-md row items-start q-gutter-md" style="justify-content: center">
        <div v-for="(article, index) in beritaList" :key="index" class="q-mb-md" style="width: 350px; margin: 19px;">
          <q-card class="my-card" clickable @click="tampilkanDetailBerita(article)" style="cursor: pointer;">
            <q-img :src="article.urlToImage" alt="">
              <div class="absolute-bottom text-h6">
                {{ article.title }}
              </div>
            </q-img>
            <q-card-section>
              {{ article.description }}
            </q-card-section>
          </q-card>
        </div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import axios from 'axios'

export default defineComponent({
  name: 'MainLayout',

  setup () {
    const beritaList = ref([])
    const searchTerm = ref('')
    const selectedCategory = ref('general')
    const categories = [
      { label: 'Umum', value: 'general' },
      { label: 'Bisnis', value: 'business' },
      { label: 'Hiburan', value: 'entertainment' },
      { label: 'Kesehatan', value: 'health' },
      { label: 'Sains', value: 'science' },
      { label: 'Olahraga', value: 'sports' },
      { label: 'Teknologi', value: 'technology' }
    ]

    const toggleSearchBar = () => {
      if (!searchTerm.value) {
        searchTerm.value = ''
      }
    }

    const tampilkanDetailBerita = (berita) => {
      window.open(berita.url)
    }

    const performSearch = async () => {
      try {
        const apiKey = '3a4b74b3ce6e407b8759445ac79d9b09'
        const apiUrl = 'https://newsapi.org/v2/everything'

        const response = await axios.get(apiUrl, {
          params: {
            q: searchTerm.value,
            apiKey
          }
        })

        beritaList.value = response.data.articles
      } catch (error) {
        console.error('Gagal melakukan pencarian:', error)
        beritaList.value = []
      }
    }

    const fetchBeritaByCategory = async (category) => {
      try {
        const apiKey = '3a4b74b3ce6e407b8759445ac79d9b09'
        const apiUrl = 'https://newsapi.org/v2/top-headlines'

        const response = await axios.get(apiUrl, {
          params: {
            country: 'US',
            category,
            apiKey
          }
        })

        beritaList.value = response.data.articles
      } catch (error) {
        console.error('Gagal mengambil berita:', error)
        beritaList.value = []
      }
    }

    const fetchCategoryBerita = async () => {
      try {
        await fetchBeritaByCategory(selectedCategory.value)
      } catch (error) {
        console.error('Gagal mengambil berita:', error)
      }
    }

    const selectCategory = async (category) => {
      selectedCategory.value = category
      await fetchBeritaByCategory(category)
    }

    onMounted(() => {
      fetchCategoryBerita()
    })

    return {
      beritaList,
      searchTerm,
      toggleSearchBar,
      tampilkanDetailBerita,
      selectedCategory,
      categories,
      performSearch,
      fetchCategoryBerita,
      selectCategory
    }
  }
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@500&display=swap');
  .font {
    font-family: 'Poppins', sans-serif;
    font-weight: 500;
    font-size: 25px;
    margin-left: 20px;
  }
  .bold-text {
  font-weight: bold;
}
</style>
