<template>
    <div>
        <h1 class="text-3xl font-bold text-center mb-6 mt-6">POKEMONS</h1>

        <div class="flex justify-center mb-6">
            <input
                v-model="search"
                type="text"
                placeholder="Pesquisar Pokémon..."
                class="px-4 py-2 border rounded-lg w-64" />
        </div>

        <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-4 gap-4">
            <PokemonCard
                v-for="p in filteredPokemons"
                :key="p.id"
                :pokemon="p"
            />
        </div>

        <div class="flex justify-center gap-4 mt-6">
            <button
                class="px-6 py-2 bg-blue-500 text-white rounded-lg shadow hover:bg-blue-600 transition-colors duration-200 disabled:bg-gray-300 disabled:cursor-not-allowed"
                @click="prevPage"
                :disabled="offset === 0"
            >
                Anterior
            </button>
            <button
                class="px-6 py-2 bg-blue-500 text-white rounded-lg shadow hover:bg-blue-600 transition-colors duration-200 disabled:bg-gray-300 disabled:cursor-not-allowed"
                @click="nextPage"
                :disabled="pokemons.length < limit"
            >
                Próximo
            </button>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, watch, computed } from 'vue'
import axios from 'axios'
import PokemonCard from '../components/PokemonCardComponent.vue'

const pokemons = ref([])
const offset = ref(0)
const limit = 12
const search = ref('')

const fetchPokemons = async () => {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?offset=${offset.value}&limit=${limit}`)
    const results = response.data.results

    const detailed = await Promise.all(
        results.map(p => axios.get(p.url).then(response => response.data))
    )

    pokemons.value = detailed.map(pokemon => ({
        id: pokemon.id,
        name: pokemon.name,
        image: pokemon.sprites.other.dream_world.front_default,
        type: pokemon.types[0].type.name
    }))
}

watch(offset, fetchPokemons)

const filteredPokemons = computed(() => {
    if (!search.value) return pokemons.value
    return pokemons.value.filter(p =>
        p.name.toLowerCase().includes(search.value.toLowerCase())
    )
})

const nextPage = () => {
    offset.value += limit
}
const prevPage = () => {
    if (offset.value >= limit) {
        offset.value -= limit
    }
}

onMounted(fetchPokemons)
</script>