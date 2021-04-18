<template>
    <div>
        <h1>Game Ended!</h1>
        <p>Player {{winner}} won!</p>

        <ScorePanel :elapsedTime="elapsedTime" :scores="scores"/>

        <button @click="$emit('startgame')">Return to lobby</button>
    </div>
</template>
<script>
import ScorePanel from './ScorePanel.vue';
import {computed} from 'vue';

export default {
    setup(props) {
        const winner = computed(() => {
            let winner = undefined;
            for (const [key, val] of Object.entries(props.scores)) {
                if (winner === undefined || val > props.scores[winner]) {
                    winner = key;
                }
            }
            return winner;
        })
        return {winner}
    },
    props: {
        elapsedTime: Number,
        scores: Object,
    },
    components: {
        ScorePanel,
    },
}

</script>
<style></style>