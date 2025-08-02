<script setup>
</script>

<template>
  <main class="flex items-center justify-center min-h-screen ">
    <div class="text-center">
      <h1 class="text-5xl font-extrabold text-indigo-700 mb-2">PokeBase</h1>
      <p class="text-lg text-gray-500 mt-2">Aprenda mais sobre seus pokemons</p>
      <router-link to="/pokedex">
        <button class="mt-6 px-8 py-3 bg-indigo-600 text-white font-semibold rounded-lg shadow-md transition transform hover:bg-indigo-700 hover:scale-105 focus:outline-none focus:ring-2 focus:ring-indigo-400 focus:ring-opacity-50 duration-200">
          Iniciar
        </button>
      </router-link>
    </div>
  </main>
</template>