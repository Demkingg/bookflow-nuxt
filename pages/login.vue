<script setup lang="ts">
const supabase = useSupabaseClient()
const email = ref('')
const password = ref('')

const signInWithPassword = async () => {
  const { error } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  })
  if (error) console.log(error)
  else navigateTo('/confirm')
}
</script>

<template>
  <div class="flex items-center justify-center h-screen">
    <form @submit.prevent="signInWithPassword" class="bg-white p-8 rounded-lg shadow-md">
      <h1 class="text-3xl font-bold mb-4">Sign in</h1>
      <div class="mb-4">
        <label for="email" class="block text-gray-700">Email</label>
        <input
          v-model="email"
          type="email"
          id="email"
          class="w-full px-4 py-2 border-gray-300 rounded-md"
          required
        />
      </div>
      <div class="mb-4">
        <label for="password" class="block text-gray-700">Password</label>
        <input
          v-model="password"
          type="password"
          id="password"
          class="w-full px-4 py-2 border-gray-300 rounded-md"
          required
        />
      </div>
      <UButton type="submit">
        Sign In
      </UButton>
      <UButton class="mt-4 ml-4" @click="navigateTo('/register')">Register</UButton>
    </form>
  </div>
</template>