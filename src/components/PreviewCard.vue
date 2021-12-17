<template>
    <div class="min-h-screen bg-gray-900 py-10">

        <div class="text-center mb-10">
            <h1 class="text-gray-50 font-semibold text-2xl mb-5">Ethereum NFT Creator</h1>
            <form @submit.prevent="createToken(name, description, eth, duration)">

                <button class="bg-teal-500 py-2 px-4 rounded mt-5 text-white font-semibold">Generate</button>
            </form>
        </div>

        <div class="p-10 grid grid-cols-3 gap-8">
            <div v-for="(token, index) in nft" :key="index" class="bg-gray-800 p-4 rounded-2xl w-64 grid gap-5 justify-center">
                <div>
                    <img class="rounded-xl" src="../../public/image-equilibrium.jpg" alt="">
                </div>

                <div class="space-y-3">
                    <h2 class="text-white text-xl font-bold capitalize">{{ token.name }} #{{ token.id }}</h2>
                    <p class="text-gray-400">{{ token.description }}</p>
                </div>

                <div class="flex justify-between">
                    <div class="flex items-center space-x-1">
                        <svg width="11" height="18" xmlns="http://www.w3.org/2000/svg"><path d="M11 10.216 5.5 18 0 10.216l5.5 3.263 5.5-3.262ZM5.5 0l5.496 9.169L5.5 12.43 0 9.17 5.5 0Z" fill="#00FFF8"/></svg>
                        <p class="text-teal-300 font-semibold">{{ token.eth }} ETH</p>
                    </div>
                    <div class="flex items-center space-x-1">
                        <svg width="17" height="17" xmlns="http://www.w3.org/2000/svg"><path d="M8.305 2.007a6.667 6.667 0 1 0 0 13.334 6.667 6.667 0 0 0 0-13.334Zm2.667 7.334H8.305a.667.667 0 0 1-.667-.667V6.007a.667.667 0 0 1 1.334 0v2h2a.667.667 0 0 1 0 1.334Z" fill="#8BACD9"/></svg>
                        <p class="text-gray-400 font-semibold">{{ token.duration }} days left</p>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script setup>
import axios from 'axios'
import { reactive, ref } from 'vue';

    const name = ref('')
    const description = ref('')
    const eth = ref(null)
    const duration = ref(null)

    const nft = reactive([])

    class NFT {
        constructor(name, id, description, eth, duration){
            this.name = name,
            this.id = id,
            this.description = description,
            this.eth = eth,
            this.duration = duration
        }
    }

    const createToken = () => {
        axios.get('https://random-word-api.herokuapp.com/word?number=1')
        .then(response => {

            const generateID = Math.floor(Math.random() * (10000 - 1000 + 1) + 1000)

            const getDuration = Math.floor(Math.random() * (10 - 2 + 1) + 2)

            const generate = Math.random() * 10

            const startingBid =  Number.parseFloat(generate).toFixed(4)

        const newNft = new NFT(response.data[0], generateID, description,startingBid, getDuration )
        nft.push(newNft)
        })        
    }

    createToken()
    



</script>