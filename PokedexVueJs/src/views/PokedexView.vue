<template>
    <div>
        <h1 class="text-3xl font-bold text-center mb-6 mt-6">Pokemons</h1>

        <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-4 gap-4">
            <PokemonCard
             v-for="p in pokemons" 
             :key="p.name"
             :pokemon="p"
             />            
        </div>

        <div class="flex justify-center gap-4 mt-6">
            <button class="btn">Anterior</button>
            <button class="btn">Próximo</button>
        </div>
    </div>

</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import PokemonCard from '../components/PokemonCardComponent.vue'

const pokemons = ref([])
const offset = ref(0)
const limit = 12

//cria função de busca de pokemons
const fetchPokemons = async() => {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?offset=0&limit=12`)
    const results = response.data.results
    console.log(results)

    //buscar detalhes de cada pokemon
    const detailed = await Promise.all(
        results.map(p => axios.get(p.url).then(response => response.data)) 
    )

    console.log(detailed)

    pokemons.value = detailed.map(pokemon => ({
        name: pokemon.name,
        image: pokemon.sprites.other.dream_world.front_default,
        type: pokemon.types[0].type.name

    }))

    console.log(pokemons)

}

onMounted(fetchPokemons)

</script>