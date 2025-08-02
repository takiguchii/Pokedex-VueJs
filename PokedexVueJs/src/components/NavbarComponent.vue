<template>
    <nav class="bg-blue-600 text-white py-4 px-8 flex justify-between items-center">
        <router-link to="/">
            <button class="text-lg font-bold focus:outline-none">Pokedex</button>
        </router-link>
        <ul class="flex gap-6">
            <li>
                <router-link to="/pokedex" class="hover:underline">Pokedex</router-link>
            </li>
            <li>
                <router-link to="/analise" class="hover:underline">Analise</router-link>
            </li>
        </ul>
    </nav>
</template>