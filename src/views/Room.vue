<template>
  <Lobby @startgame="startgame" v-if="state == GAME_STATES.LOBBY"/>
  <Game @gameover="gameover" v-if="state == GAME_STATES.GAME_ACTIVE" :numDots="numDots" ref="game"/>
  <GameOver @rejoinLobby="rejoinLobby" :elapsedTime="elapsedTime" :scores="scores" v-if="state == GAME_STATES.GAME_OVER"/>
</template>

<script>
import { ref } from 'vue';
import Lobby from '@/components/Lobby.vue'
import Game from '@/components/Game.vue'
import GameOver from '@/components/GameOver.vue'

const GAME_STATES = {
  LOBBY: 1,
  GAME_ACTIVE: 2,
  GAME_OVER: 3,
}

export default {
  name: 'Room',
  setup() {
    const state = ref(GAME_STATES.LOBBY);
    const numDots = ref(0)
    return {numDots, state, GAME_STATES}
  },
  methods: {
    gameover(time, scores) {
      this.state = GAME_STATES.GAME_OVER;
      this.elapsedTime = time;
      this.scores = scores;
    },
    rejoinLobby() {
      this.state = GAME_STATES.LOBBY;
    },
    startgame(numDots) {
      this.state = GAME_STATES.GAME_ACTIVE;
      this.numDots = numDots;
    }
  },
  components: {
    Lobby,
    Game,
    GameOver,
  }

}
</script>

<style>

</style>

