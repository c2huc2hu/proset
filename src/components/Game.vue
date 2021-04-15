<template>
    <div class="playing-field">
        <transition-group name="animate-card">
            <template v-for="(card, i) in visibleCards" >
                <Card :value="card" v-if="card != null" :key="card" @click="toggleSelect(i)" :selected="selected.has(i)"/>
            </template>
        </transition-group>
    </div>


    <button @click="testRemoveCards">Remove</button>
    <button @click="submit">Submit</button>

</template>

<script>
import {reactive} from 'vue';
import Card from './Card.vue';

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
    setup() {
        const cards = reactive([]);
        for (let i=1; i<8; i++) {
            cards.push(i);
        }
        shuffle(cards);

        const visibleCards = reactive([]);
        const selected = reactive(new Set())

        return {cards, visibleCards, selected}
    },
    mounted() {
        this.dealNewCards();
    },
    methods: {
        dealNewCards() {
            while (this.cards.length && this.visibleCards.length < 7) {
                this.visibleCards.push(this.cards.pop());
            }
        },
        gameOver() {
            // TODO: Make the last set of cards not count
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
        },
        replaceCards(indices) {
            indices.map((x, i) => {
                this.visibleCards.splice(x, 1, this.cards.pop())
            })
        },
        submit() {
            const selectedCards = Array.from(this.selected).map(index => this.visibleCards[index]);
            console.log(selectedCards)

            if (checkSet(selectedCards)) {
                this.replaceCards(Array.from(this.selected))
                this.selected.clear()
            }
            else {
                console.log('Not a set');
            }
        },
    },
    components: {
        Card,
    }
}
</script>

<style>
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
    transition: all 0.5s ease;
    height: 100%;
    flex-basis: 150px;
}
</style>