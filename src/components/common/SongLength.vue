<template>
    <span class="songLength">
        {{ this.invalid ? "-" : formatTime(this.seconds) }}
    </span>
</template>

<script>
export default {
    name: 'SongLength',
    props: {
        seconds: Number,
        invalid: Boolean,
    },
    methods: {
        formatTime(seconds) {
            const minutes = Math.floor(seconds / 60)
            seconds = Math.floor(seconds % 60)
            if (seconds < 10) {
                seconds = `0${seconds}`
            }

            return `${minutes}:${seconds}`
        },
    },
};
</script>

<style scoped lang="scss">
.songLength {
    height: 3em;
    line-height: 3em;
}
</style>