<template>
    <div v-if="pokemon" class="container mx-auto p-4">
        <div class="bg-white rounded-lg shadow-lg p-6">
            <div class="flex items-center mb-6">
                <img :src="pokemon.image" :alt="pokemon.name" class="w-32 h-32 mr-6">
                <div>
                    <h1 class="text-3xl font-bold capitalize">{{ pokemon.name }}</h1>
                    <span class="text-gray-600 capitalize">{{ pokemon.type }}</span>
                </div>
            </div>
            <div class="w-full h-[400px] max-w-2xl mx-auto">
                <PokemonRadarChart v-if="pokemon.stats" :chartData="chartData" />
            </div>
        </div>
    </div>
    <div v-else class="text-center p-4">
        Carregando...
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
            backgroundColor: 'rgba(54, 162, 235, 0.2)',
            borderColor: 'rgb(54, 162, 235)',
            pointBackgroundColor: 'rgb(54, 162, 235)',
            pointBorderColor: '#fff',
            pointHoverBackgroundColor: '#fff',
            pointHoverBorderColor: 'rgb(54, 162, 235)'
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