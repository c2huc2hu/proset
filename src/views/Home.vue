<template>
  <div class="home">
    <h1>ProSet</h1>

    <p>Find sets of 3+ cards that have an even number of each colour of dot.</p>
    <div class="example set">
        <div class="card-set">
            <Card :value="3"/>
            <Card :value="5"/>
            <Card :value="6"/>
        </div>
        <p>This is a set</p>
    </div>
    <div class="example not-set">
        <div class="card-set">
            <Card :value="10"/>
            <Card :value="15"/>
            <Card :value="4"/>
        </div>
        <p>This is not a set</p>
    </div>

    <p><a href="https://en.wikipedia.org/wiki/Projective_Set_(game)">More info (Wikipedia)</a></p>
  </div>
</template>

<script>
import Card from '@/components/Card.vue'

export default {
  name: 'Home',
  components: {Card}
}
</script>

<style scoped>
.example {
    display: inline-block;
    border: solid 2px;
    margin: 10px;
}

.example.set {
    border-color: green;
}
.example.not-set {
    border-color: red;
}
.example p {
    margin: 0;
    font-weight: bold;
    font-size: 1.2em;
}

.card-set {
    padding: 0 10px;
    margin: 0 10px;
    min-height: 0;
}

.card-set > .card {
    width: 15vmin;
}
</style>