<template>
  <footer class="bg-gray-800 text-white py-6 fixed bottom-0 left-0 right-0">
    <div class="container mx-auto flex flex-col md:flex-row justify-between items-center">
      <!-- Copyright -->
      <div class="text-sm">
        &copy; 2024 My Nuxt App. All rights reserved.
      </div>

      <!-- Footer Links -->
      <nav class="mt-4 md:mt-0">
        <ul class="flex space-x-4">
          <li>
            <NuxtLink to="/" class="hover:underline">Home</NuxtLink>
          </li>
          <li>
            <NuxtLink to="/about" class="hover:underline">About</NuxtLink>
          </li>
        </ul>
      </nav>
    </div>
  </footer>
</template>

