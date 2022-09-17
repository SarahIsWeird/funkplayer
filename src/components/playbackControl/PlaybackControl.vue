<template>
    <div class="playbackControl">
        <span @click="rewindClicked" class="material-symbols-outlined">fast_rewind</span>
        <span v-if="isPlaying" @click="playClicked" class="material-symbols-outlined">pause</span>
        <span v-else @click="playClicked" class="material-symbols-outlined">play_arrow</span>
        <span @click="fastForwardClicked" class="material-symbols-outlined">fast_forward</span>
    </div>
</template>

<script>
export default {
    name: 'PlaybackControl',
    props: {
        isPlaying: Boolean,
        onRewindClicked: Function,
        onPlayClicked: Function,
        onFastForwardClicked: Function,
    },
    methods: {
        rewindClicked(e) {
            e.preventDefault()

            this.$emit('rewindClicked');
        },
        playClicked(e) {
            e.preventDefault()

            this.$emit('playClicked');
        },
        fastForwardClicked(e) {
            e.preventDefault()

            this.$emit('fastForwardClicked');
        }
    },
};
</script>

<style lang="scss">
.playbackControl {
    height: 3em;

    span {
        line-height: 2em;
        vertical-align: middle;

        padding-left: 0.125em;

        cursor: pointer;
    }
}
</style>