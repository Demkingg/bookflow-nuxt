<script setup lang="ts">

const user = useSupabaseUser()

const loggedIn = computed(() => !!user.value)
const supabase = useSupabaseClient()
const signOut = async () => {
  const { error } = await supabase.auth.signOut()
  if (error) console.log(error)
  else navigateTo('/login')
}

</script>

<template>
  <header class="bg-gray-800 text-white shadow-md">
    <div class="container mx-auto flex justify-between items-center py-4">
      <!-- Logo -->
      <div class="text-lg font-bold">
        <NuxtLink to="/">B00kfl0w</NuxtLink>
      </div>
      
      <!-- Navigation Links -->
      <nav>
        <ul class="flex space-x-4">
          <li>
            <UButton><NuxtLink to="/readings">Readings</NuxtLink></UButton>
          </li>
          <li>
            <UButton><NuxtLink to="/about">About</NuxtLink></UButton>
          </li>
          <li v-if="loggedIn">
            <UButton @click="signOut">Sign Out</UButton>
          </li>
          <li v-else>
            <UButton><NuxtLink to="/login">Sign In</NuxtLink></UButton>
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>
