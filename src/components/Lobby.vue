<template>
    <div>
        <h1>Game</h1>
        <h2>Difficulty</h2>
        <button class="button" @click="$emit('startgame', 3)">Beginner</button>
        <button class="button" @click="$emit('startgame', 4)">Easy</button>
        <button class="button" @click="$emit('startgame', 6)">Hard</button>
    </div>
</template>

<script></script>

<style>
</style>