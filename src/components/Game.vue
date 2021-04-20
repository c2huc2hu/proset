<template>
    <div class="game-container">
        <div class="game">
            <div class="playing-field">
                <transition-group name="animate-card">
                    <template v-for="(card, i) in visibleCards">
                        <Card :value="card" v-if="card != null" :key="card || i" @click="toggleSelect(i)" :selected="selected.has(i)"/>
                    </template>
                </transition-group>

            </div>

            <p>{{cards.length}} cards remaining</p>

            <button class="wide-button" @click="testRemoveCards">Remove a card (testing purposes)</button>
            <button @click="submit">Submit</button>
        </div>
        <ScorePanel :scores="scores" :elapsedTime="elapsedTime"/>
    </div>
</template>

<script>
const EMPTY_CARD = 0; // use 0 to represent a lack of card when the deck is empty

import {reactive, ref} from 'vue';
import Card from './Card.vue';
import ScorePanel from './ScorePanel.vue';


// In-place shuffle, https://stackoverflow.com/questions/2450954/
function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}
function checkSet(cards) {
    if (!cards.length) {
        return false;
    }
    return !!(cards.reduce((x, y) => x ^ y) === 0);
}

export default {
    name: "Game",
    props: {
        numDots: Number,
    },
    setup() {
        const cards = reactive([]);
        const visibleCards = reactive([]);
        const selected = reactive(new Set());
        const startTime = Date.now();
        const elapsedTime = ref(0);

        const scores = reactive({'me': 0});

        return {cards, visibleCards, selected, startTime, elapsedTime, scores};
    },
    mounted() {
        this.reset();
        this.interval = setInterval(() => {
            this.elapsedTime = (Date.now() - this.startTime);
        }, 1000);
    },
    unmount() {
        clearInterval(this.interval)
    },
    methods: {
        reset() {
            if (this.cards.length !== 0 || this.visibleCards.length !== 0) {
                alert('Something went wrong when resetting');
                this.cards.splice(0);
                this.visibleCards.splice(0);
            }

            for (let i=1; i<2**this.numDots; i++) {
                this.cards.push(i);
            }
            shuffle(this.cards);
            while (this.cards.length && this.visibleCards.length < this.numDots + 1) {
                this.visibleCards.push(this.cards.pop());
            }
        },
        gameIsOver() {
            // If there are no cards left in the deck, choosing everything will yield a set.
            // This is just a clicking race, so don't count it and end the game early
            return this.cards.length === 0;
        },
        toggleSelect(index) {
            if (this.selected.has(index)) {
                this.selected.delete(index);
            }
            else {
                this.selected.add(index);
            }
        },
        testRemoveCards() {
            // TODO: Remove this function
            this.replaceCards([2])
            console.log(...this.visibleCards)
        },
        replaceCards(indices) {
            indices.forEach(x => {
                if (this.visibleCards[x] != EMPTY_CARD) {
                    this.visibleCards.splice(x, 1, this.cards.pop() || EMPTY_CARD)
                }
            })
        },
        submit() {
            const selectedCards = Array.from(this.selected).map(index => this.visibleCards[index]).filter(x => x !== EMPTY_CARD);

            if (checkSet(selectedCards)) {
                this.scores['me'] += 1;
                this.replaceCards(Array.from(this.selected))
                this.selected.clear()

                if (this.gameIsOver()) {
                    this.$emit('gameover', this.elapsedTime, this.scores);
                }
            }
            else {
                console.log('Not a set');
            }
        },
    },
    components: {
        Card,
        ScorePanel,
    }
}
</script>

<style>
.game-container {
    display: flex;
}
.playing-field {
    display: flex;
    flex-wrap: wrap;
    position: relative;
    max-height: 100%;
}

.animate-card-enter-from {
    opacity: 0;
    transform: translateX(300px);
}
.animate-card-leave-active {
    position: absolute;
    left: -200px;
}
.animate-card-leave-to {
    opacity: 0;
}
.animate-card-move {
    z-index: 1;
}

.card {
    width: 25%;
    transition: all 0.5s ease;
    max-height: 40vmin;
}

.wide-button {
    width: 300px;
}
</style>