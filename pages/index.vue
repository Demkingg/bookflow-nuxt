<script setup>
const client = useSupabaseClient()
const user = useSupabaseUser()

const query = ref('')
const { data: books, refresh } = await useFetch('https://www.googleapis.com/books/v1/volumes', {
  query: {
    q: query
  }
})

watch(query, async (newQuery) => {
  await refresh()
})

async function insertReading(bookname, bookid, imageurl, userid) {
  const { data, error } = await client.from('readings').insert({ bookname: bookname, bookid: bookid, imageurl: imageurl, userid: userid }).select()
  return data
}

</script>

<template>
  <div class="container mx-auto p-8">
    <h1 class="text-3xl font-bold mb-6">Book Search</h1>
<span>
    <UForm @submit.prevent="refresh" class="mb-6">
      <input
        v-model="query"
        type="search"
        placeholder="Search for a book..."
        class="px-4 py-2 border-gray-300 rounded-md"
      />
    </UForm>
</span>
    <!-- Check if books data is available -->
    <div v-if="books" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div v-for="(book, index) in books.items" :key="index" class="bg-white shadow-md rounded-lg p-4">
        <img :src="book.volumeInfo.imageLinks?.thumbnail" alt="Book cover" class="mb-4 w-32 rounded">
        <h2 class="text-xl font-semibold mb-2">{{ book.volumeInfo.title }}</h2>
        <p class="text-gray-700 mb-2">By: {{ book.volumeInfo.authors?.join(', ') }}</p>
        <p class="text-gray-500 text-sm mb-4">{{ book.volumeInfo.publishedDate }}</p>
        <p class="text-gray-700 mb-4 line-clamp-3">{{ book.volumeInfo.description }}</p>
        <a
          :href="book.volumeInfo.infoLink"
          class="text-blue-600 hover:underline"
          target="_blank"
          rel="noopener noreferrer"
        >More Info</a>
        <UButton @click="navigateTo(`/book/${book.id}`)" class="mt-4 ml-2 bottom-0">Open book</UButton>
        <UButton @click="insertReading(book.volumeInfo.title, book.id, book.volumeInfo.imageLinks?.thumbnail, user.id)" class="mt-4 ml-2 bottom-0">Add to Reading List</UButton>
        </div>
    </div>

    <div v-else class="text-center">
      <p>No book found.</p>
    </div>
  </div>
</template>

