<template>
    <img class="object-cover w-full h-28" :src="reading?.imageurl" :alt="reading?.bookname" />
    <div class="container mx-auto py-12">
        <div class="px-6">
            <h1 class="text-3xl font-bold mb-4">{{ reading?.bookname }}</h1>
            <p class="text-lg">Id: {{ reading?.id }}</p>
            <p class="text-lg">Notes: {{ reading?.notes }}</p>
            <p class="text-lg">Rating: {{ reading?.rating }}</p>
            <p class="text-lg">Read complete: {{ reading?.readcomplete }}</p>
            <p class="text-lg">Read start: {{ reading?.readstart }}</p>
            <UButton @click="deleteReading" color="red" variant="soft" class="mt-4">Remove reading</UButton>
          </div>
    </div>
</template>
  
<script setup lang="ts">
  const route = useRoute()
  
  const client = useSupabaseClient()
  const { data: reading } = await useAsyncData('reading', async () => {
    const { data } = await client.from('readings').select('bookname, id, notes, rating, readcomplete, readstart, imageurl').eq('id', route.params.id).single()
    return data
  })

  async function deleteReading() {
    const { error } = await client.from('readings').delete().eq('id', route.params.id)
    if (error) console.log(error)
    else navigateTo('/readings')
  }
</script>