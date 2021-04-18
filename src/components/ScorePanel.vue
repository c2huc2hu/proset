<template>
    <div class="score-panel">
        <div class="timer"> {{ timerString }} </div>
        <table class="player-list">
            <tr v-for="player in sortedPlayers">
                <td class="score-panel-cell"> {{player}} </td>
                <td class="score-panel-cell"> {{scores[player]}} </td>
            </tr>
        </table>
    </div>
</template>

<script>
import {computed, reactive, ref} from 'vue';

// Convert a time like 183 to 03:03
// https://stackoverflow.com/a/40350003
function formatTime(seconds) {
  const h = Math.floor(seconds / 3600);
  const m = Math.floor((seconds % 3600) / 60);
  const s = Math.round(seconds % 60);
  return [
    h,
    m > 9 ? m : (h ? '0' + m : m || '0'),
    s > 9 ? s : '0' + s
  ].filter(Boolean).join(':');
}

export default {
    props: {
        elapsedTime: Number, // in milliseconds
        currentPlayer: String,
        scores: Object, // player -> score
    },
    setup(props) {
        const timerString = computed(() => formatTime(props.elapsedTime / 1000))
        const sortedPlayers = Object.keys(props.scores).sort(player => scores[player])

        return {timerString, sortedPlayers}
    },
    methods: {
    }
}
</script>


<style>
.timer {
    font-size: 3em;
}
.player-list {
    width: 100%;
    margin-bottom: 20px 0;
}
.score-panel-cell {
    /*width: 200px;*/
    border: solid 1px #aaa;
    font-size: 1.5em;
}
</style>