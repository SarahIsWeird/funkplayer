<template>
    <MusicPlayer />
</template>

<script>
import MusicPlayer from './components/MusicPlayer.vue'

export default {
    name: 'App',
    components: {
        MusicPlayer
    }
}
</script>

<style lang="scss">
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;

    cursor: default;
    -webkit-user-select: none;
    user-select: none;
}

* {
    margin: 0;
    padding: 0;
}

.material-symbols-outlined {
    font-variation-settings:
        'FILL' 1,
        'wght' 400,
        'GRAD' 0,
        'opsz' 48
}
</style>
