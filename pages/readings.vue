<template>
   <section class="bg-white py-12">
    <div v-if="reading" class="container mx-auto grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
      <div v-for="readingItem in reading" :key="readingItem.id" class="bg-white shadow-md rounded-lg p-4 min-w-[300px]">
          <div class="flex-shrink-0">
            <img class="w-32" :src="readingItem.imageurl" :alt="readingItem.bookname">
          </div>
          <div class="w-full">
            <h2 class="text-2xl font-bold">{{ readingItem.bookname }}</h2>
            <p class="text-gray-600">Rating: {{ readingItem.rating }}</p>
            <p class="text-gray-600">Read Count: {{ readingItem.readcount }}</p>
            <p class="text-gray-600">Read Complete: {{ readingItem.readcomplete }}</p>
            <p class="text-gray-600">Read Start: {{ readingItem.readstart }}</p>
            <p class="text-gray-600">{{ readingItem.notes }}</p>
            <UButton @click="navigateTo(`/reading/${readingItem.id}`)" class="mt-4 bottom-0">Open reading</UButton>
          </div>
        </div>
      </div>
  </section>
</template>

<script setup lang="ts">
const client = useSupabaseClient()
const user = useSupabaseUser()

const { data: reading } = await useAsyncData<{ id: number, bookname: string, bookid: string, readcount: number, rating: number, notes: string, readcomplete: string, readstart: string, imageurl: string }[]>('reading', async () => {

  const { data } = await client.from('readings').select('*').eq('userid', user.value.id)

  return data
})

</script>

