<template>
    <div class="rounded-lg p-4 text-white shadow-md bg-gray-300 hover:bg-gray-400 cursor-pointer" @click="goToAnalise">
        <img :src="pokemon.image" :alt="pokemon.name" class="mx-auto mb-2 h-20">
        <h2 class="text-center capitalize font-bold">{{ pokemon.name }}</h2>
        <p class="text-center text-sm uppercase">{{ pokemon.type }}</p>
    </div>
</template>

<script setup>
import { useRouter } from 'vue-router'

const router = useRouter()
const props = defineProps({
    pokemon: Object
})

const goToAnalise = () => {
    router.push(`/analise/${props.pokemon.id}`)
}
</script>