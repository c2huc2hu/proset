<template>
  <div class="home">
    <h1>ProSet</h1>

    <p>Find sets of 3+ cards that have an even number of each colour of dot.</p>
    <div class="example">
        <div class="card-set set">
            <Card :value="3"/>
            <Card :value="5"/>
            <Card :value="6"/>
        </div>
        <p>This is a set</p>
    </div>
    <div class="example not-set">
        <div class="card-set not-set">
            <Card :value="10"/>
            <Card :value="15"/>
            <Card :value="4"/>
        </div>
        <p>This is not a set: it has one red dot</p>
    </div>
    <div class="example">
        <div class="card-set set">
            <Card :value="12"/>
            <Card :value="25"/>
            <Card :value="34"/>
            <Card :value="10"/>
            <Card :value="61"/>
        </div>
        <p>This is also a set</p>
    </div>

    <p>There is guaranteed to be at least one set in a seven-card deal.</p>
    <p>The game ends when there are no cards that haven't been dealt.</p>
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
    margin: 10px;
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
.card-set.set {
    border: solid 2px green;
}
.card-set.not-set {
    border: solid 2px red;
}

.card-set > .card {
    width: 10vmin;
}
</style>