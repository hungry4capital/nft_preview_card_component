<template>
    <div class="min-h-screen bg-gray-900">

        <div class="text-center mb-10">
            <h1 class="text-gray-50 font-semibold text-2xl mb-3">Ethereum NFT Card Generator</h1>
                
                <div class="space-x-2">
                    <button @click="createToken" class="bg-teal-500 py-2 px-4 rounded mt-2 text-white font-semibold">Generate</button>
                    <button @click.prevent="reset" class="bg-teal-500 py-2 px-4 rounded mt-2 text-white font-semibold">Reset</button>
                </div>

                
        </div>
        
        <div class="rounded-lg my-8 max-w-2xl mx-auto py-8 px-4 sm:px-6 md:max-w-3xl lg:max-w-5xl lg:px-8 shadow-xl">
            <div class="p-5 max-w-6xl grid grid-cols-1 place-items-center md:grid-cols-2 lg:grid-cols-3 gap-8">
                <div v-for="(token, index) in nft" :key="index" class="bg-gray-800 p-4 rounded-2xl w-64 grid gap-5 justify-center">
                    <div>
                        <img class="rounded-xl" src="../../public/image-equilibrium.jpg" alt="">
                    </div>

                    <div class="space-y-3">
                        <h2 class="text-white text-xl font-bold capitalize">{{ token.name }} #{{ token.id }}</h2>
                        <div class="max-h-16 overflow-clip hover:overflow-auto">
                            <p class="text-gray-400">{{ token.description }}</p>
                        </div>
                        
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

        

    </div>
</template>

<script setup>
import axios from 'axios'
import { reactive, ref } from 'vue';
import paragraph from '../composables/getTokenTitle'

    let nft = reactive([])

    class NFT {
        constructor(name, id, description, eth, duration){
            this.name = name,
            this.id = id,
            this.description = description,
            this.eth = eth,
            this.duration = duration
        }
    }

    const tokenTitle = ref('')

    const createToken = async () => {       

            if(nft.length < 6 && nft.length >= 0) {
                try {
                    const getToken = await axios.get(
                    "https://random-word-api.herokuapp.com/word?number=1&swear=0"
                    );

                    tokenTitle.value = getToken.data[0]

                } catch (e) {
                    console.log(e);
                } 

                const generateID = Math.floor(Math.random() * (10000 - 1000 + 1) + 1000)

                const getDuration = Math.floor(Math.random() * (10 - 2 + 1) + 2)

                const generate = Math.random() * 10

                const startingBid =  Number.parseFloat(generate).toFixed(4)

                const getParagraph = paragraph[Math.floor(Math.random() * (10 - 1) + 1)]

                const newNft = new NFT(tokenTitle.value, generateID, getParagraph,startingBid, getDuration )
                nft.push(newNft)
        }    
    }

    const reset = () => {
        console.log(nft.length)
        nft.length = 0
        createToken()
    }
    createToken()
    



</script>