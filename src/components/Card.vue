<template>
    <svg :class="{'card': true, selected}" viewBox="0 0 100 150">
        <rect width="90" height="140" x="5" y="5" rx="10" ry="10" fill="white" stroke="black" stroke-width="1.5" class="card-border" v-if="value != 0"/>
        <template v-for="(color, i) in colors">
            <circle :cx="(i % 2) * 50 + 25" :cy="Math.floor(i / 2) * 50 + 25" r="15" :fill="color" stroke="black" :key="i" stroke-width="1" v-if="(value >> i) % 2"/>
        </template>
    </svg>
</template>

<script>
// import {reactive} from 'vue';

export default {
    name: 'Card',
    props: {
        selected: Boolean,
        value: Number,
    },
    setup() {
        const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'purple'];
        return {colors};
    },
}
</script>

<style>
.selected .card-border {
    stroke: gold;
}
</style>