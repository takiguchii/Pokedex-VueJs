<template>
    <div v-if="pokemon" class="container mx-auto p-4 flex justify-center items-center min-h-screen ">
        <div class="bg-white/90 rounded-3xl shadow-2xl p-8 max-w-3xl w-full border border-indigo-100">
            <div class="flex flex-col md:flex-row items-center mb-8 gap-8">
                <div class="relative">
                    <img :src="pokemon.image" :alt="pokemon.name" class="w-40 h-40 drop-shadow-lg rounded-full border-4 border-indigo-200 bg-white" />
                    <span class="absolute -bottom-3 left-1/2 -translate-x-1/2 px-4 py-1 bg-indigo-500 text-white text-xs rounded-full shadow font-semibold capitalize">
                        {{ pokemon.type }}
                    </span>
                </div>
                <div class="text-center md:text-left">
                    <h1 class="text-4xl font-extrabold capitalize text-indigo-700 drop-shadow mb-2">{{ pokemon.name }}</h1>
                    <p class="text-gray-500 text-lg">#{{ pokemon.id }}</p>
                </div>
            </div>
            <div class="w-full h-[400px] max-w-2xl mx-auto bg-gradient-to-tr from-indigo-50 to-blue-50 rounded-2xl shadow-inner p-4">
                <PokemonRadarChart v-if="pokemon.stats" :chartData="chartData" />
            </div>
        </div>
    </div>
    <div v-else class="flex justify-center items-center min-h-screen bg-gradient-to-br from-blue-100 via-indigo-100 to-purple-100">
        <span class="text-indigo-600 text-xl font-semibold animate-pulse">Carregando...</span>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'
import { useRoute } from 'vue-router'
import axios from 'axios'
import PokemonRadarChart from '../components/PokemonRadarChart.vue'

const route = useRoute()
const pokemon = ref(null)

const chartData = computed(() => {
    if (!pokemon.value?.stats) return null

    return {
        labels: ['HP', 'Attack', 'Defense', 'Sp. Attack', 'Sp. Defense', 'Speed'],
        datasets: [{
            label: pokemon.value.name,
            data: pokemon.value.stats,
            fill: true,
            backgroundColor: 'rgba(99, 102, 241, 0.2)',
            borderColor: 'rgb(99, 102, 241)',
            pointBackgroundColor: 'rgb(99, 102, 241)',
            pointBorderColor: '#fff',
            pointHoverBackgroundColor: '#fff',
            pointHoverBorderColor: 'rgb(99, 102, 241)'
        }]
    }
})

const fetchPokemon = async () => {
    try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${route.params.id}`)
        pokemon.value = {
            id: response.data.id,
            name: response.data.name,
            image: response.data.sprites.other.dream_world.front_default,
            type: response.data.types[0].type.name,
            stats: response.data.stats.map(stat => stat.base_stat)
        }
    } catch (error) {
        console.error('Error fetching pokemon:', error)
    }
}

onMounted(fetchPokemon)
</script>