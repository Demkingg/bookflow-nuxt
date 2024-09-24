<template>
  <div v-if="book" class="h-screen bg-gray-100">
    <div class="container mx-auto py-12 px-4 md:px-6">
      <div class="bg-white rounded-lg shadow-md p-6 md:p-12" :style="{ 'background-image': `url(${book?.items?.[0]?.volumeInfo?.imageLinks?.thumbnail})`, 'background-size': 'contain', 'background-position': 'right center', 'background-repeat': 'no-repeat', 'max-height': '300px' }">
        <h1 class="text-4xl font-bold">{{ book?.items?.[0]?.volumeInfo?.title }}</h1>
        <p class="text-2xl">{{ book?.items?.[0]?.volumeInfo?.subtitle }}</p>
        <p class="text-lg">Authors: {{ book?.items?.[0]?.volumeInfo?.authors?.join(', ') }}</p>
        <p class="text-lg">Published Date: {{ book?.items?.[0]?.volumeInfo?.publishedDate }}</p>
        <p class="text-lg">Page Count: {{ book?.items?.[0]?.volumeInfo?.pageCount }}</p>
        <p class="text-lg">Language: {{ book?.items?.[0]?.volumeInfo?.language }}</p>
        <p><a class="text-blue-500 hover:text-blue-700 underline" :href="book?.items?.[0]?.volumeInfo?.previewLink" target="_blank">Preview Link</a></p>
        <p><a class="text-blue-500 hover:text-blue-700 underline" :href="book?.items?.[0]?.volumeInfo?.infoLink" target="_blank">Info Link</a></p>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
  const route = useRoute()
  
  const { data: book } = await useFetch<{ items: { volumeInfo: { title: string, subtitle: string, authors: string[], publishedDate: string, pageCount: number, language: string, imageLinks: { thumbnail: string }, previewLink: string, infoLink: string } }[] }>('https://www.googleapis.com/books/v1/volumes', {
  query: {
    q: route.params.id
  }
})

console.log(book)

</script>

