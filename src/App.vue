<template>
    <div class="p-6">
        <div class="grid grid-cols-2 gap-4 mb-4">
            <button @click="currentPlayerIs(1)" :class="[currentPlayer === 1 ? 'border border-1 border-gray-500 bg-green-50' : 'text-white']" class="rounded-lg p-4">
                <span class="block text-4xl">{{ players[1].score }}</span>
                <span class="block">{{ players[1].name }}</span>
                <span class="block">HB: {{ players[1].highestBreak }}</span>
            </button>
            <button @click="currentPlayerIs(2)" :class="[currentPlayer === 2 ? 'border border-1 border-gray-500 bg-green-50' : 'text-white']" class="rounded-lg p-4">
                <span class="block text-4xl">{{ players[2].score }}</span>
                <span class="block">{{ players[2].name }}</span>
                <span class="block">HB: {{ players[2].highestBreak }}</span>
            </button>
        </div>
        <div class="space-y-4">
            <div class="grid grid-cols-2 gap-4">
                <div class="space-y-4">
                    <div class="grid grid-cols-2 gap-4">
                        <button @click="add(1)" class="w-full h-16 bg-red-600 rounded-lg flex items-center justify-center mr-4">
                            <span class="text-lg text-white">1</span>
                        </button>
                        <button @click="add(7)" class="w-full h-16 bg-black rounded-lg flex items-center justify-center">
                            <span class="text-lg text-white">7</span>
                        </button>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <button @click="add(5)" class="w-full h-16 bg-blue-600 rounded-lg flex items-center justify-center">
                            <span class="text-lg text-white">5</span>
                        </button>
                        <button @click="add(6)" class="w-full h-16 bg-pink-600 rounded-lg flex items-center justify-center">
                            <span class="text-lg text-white">6</span>
                        </button>
                    </div>
                </div>
                <div class="flex items-center justify-center">
                    <div class="text-center text-white">
                        <div class="text-4xl">{{ currentBreak }}</div>
                        <div>Break</div>
                    </div>
                </div>
            </div>
            <div class="grid grid-cols-4 gap-4">
                <button @click="add(3)" class="w-full h-16 bg-green-600 rounded-lg flex items-center justify-center">
                    <span class="text-lg text-white">3</span>
                </button>
                <button @click="add(4)" class="w-full h-16 bg-yellow-900 rounded-lg flex items-center justify-center">
                    <span class="text-lg text-white">4</span>
                </button>
                <button @click="add(2)" class="w-full h-16 bg-yellow-400 rounded-lg flex items-center justify-center">
                    <span class="text-lg text-white">2</span>
                </button>
                <div></div>
            </div>
            <div class="grid grid-cols-4 gap-4">
                <button @click="foul(4)" class="w-full h-16 bg-gray-100 rounded-lg flex items-center justify-center">
                    <span class="text-lg text-red-600">4</span>
                </button>
                <button @click="foul(5)" class="w-full h-16 bg-gray-100 rounded-lg flex items-center justify-center">
                    <span class="text-lg text-red-600">5</span>
                </button>
                <button @click="foul(6)" class="w-full h-16 bg-gray-100 rounded-lg flex items-center justify-center">
                    <span class="text-lg text-red-600">6</span>
                </button>
                <button @click="foul(7)" class="w-full h-16 bg-gray-100 rounded-lg flex items-center justify-center">
                    <span class="text-lg text-red-600">7</span>
                </button>
            </div>
            <div class="grid grid-cols-4 gap-4">
                <button @click="clear()" class="w-full h-16 bg-gray-100 rounded-lg flex items-center justify-center">
                    <span class="text-lg">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-red-600">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M9.75 9.75l4.5 4.5m0-4.5l-4.5 4.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </span>
                </button>
            </div>
            <div class="bg-gray-50 rounded-lg p-4">
                <div class="overflow-y-auto h-36 ">
                    <div v-for="log in logs" class="text-xs">
                        {{ log }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            currentPlayer: 1,
            currentBreak: 0,
            logs: [],
            players: {
                1: {
                    name: "Player 1",
                    score: 0,
                    highestBreak: 0
                },
                2: {
                    name: "Player 2",
                    score: 0,
                    highestBreak: 0
                }
            }
        }
    },
    mounted() {
        this.logs.unshift(`${new Date().toLocaleTimeString()} Started new frame.`);
    },
    methods: {
        clear() {
            let winner = this.players[1].score > this.players[2].score ? 1 : 2;
            this.logs.unshift(`${new Date().toLocaleTimeString()} Frame ended. Player ${winner} won with a score of ${this.players[winner].score}.`);
            this.currentPlayer = 1;
            this.currentBreak = 0;
            this.players = {
                1: {
                    name: "Player 1",
                    score: 0,
                    highestBreak: 0
                },
                2: {
                    name: "Player 2",
                    score: 0,
                    highestBreak: 0
                }
            }
            this.logs.unshift(`${new Date().toLocaleTimeString()} Started new frame.`);
        },
        foul(value) {
            this.logs.unshift(`${new Date().toLocaleTimeString()} Player ${this.currentPlayer} fouled. ${value} points to Player ${this.currentPlayer === 1 ? 2 : 1}.`);
            this.currentPlayerIs(this.currentPlayer === 1 ? 2 : 1)
            this.players[this.currentPlayer].score += value;
        },
        currentPlayerIs(value) {
            this.logs.unshift(`${new Date().toLocaleTimeString()} End of break [${this.currentBreak}] for Player ${this.currentPlayer}.`);
            if(this.players[this.currentPlayer].highestBreak < this.currentBreak) {
                this.players[this.currentPlayer].highestBreak = this.currentBreak;
                this.logs.unshift(`${new Date().toLocaleTimeString()} Player ${this.currentPlayer}: New high break set [${this.currentBreak}].`);
            }
            this.currentBreak = 0;
            this.logs.unshift(`${new Date().toLocaleTimeString()} Player switched to Player ${value}.`);
            this.currentPlayer = value;
        },
        add(value) {
            this.currentBreak += value;
            this.players[this.currentPlayer].score += value;
        }
    }
}
</script>
