<template>
    <div v-if="song !== undefined" class="controlBar">
        <SongInfo :song-name="song.name" :song-artist="song.artist" />
        <input
            class="progressSlider"
            type="range"
            min="0"
            :max="song.duration"
            step="0.1"
            :value="currentTime"
            @input="$emit('skipTo', $event.target.value)" />
        <div>
            <SongLength :seconds="currentTime" :invalid="false" />
            /
            <SongLength :seconds="song.duration" :invalid="false" />
        </div>
        <PlaybackControl
            @rewindClicked="onRewindClicked"
            @playClicked="onPlayClicked"
            @fastForwardClicked="onFastForwardClicked"
            :is-playing="isPlaying" />
        <VolumeControl :volume="volume" @volumeChanged="vol => $emit('volumeChanged', vol)" />
    </div>
</template>

<script>
import SongInfo from '@/components/common/SongInfo';
import PlaybackControl from '@/components/playbackControl/PlaybackControl';
import VolumeControl from '@/components/playbackControl/VolumeControl';
import SongLength from '@/components/common/SongLength';

export default {
    name: 'ControlBar',
    components: { SongLength, VolumeControl, PlaybackControl, SongInfo },
    props: {
        song: Object,
        currentTime: Number,
        volume: Number,
        onVolumeChanged: Function,
        isPlaying: Boolean,
        onRewindClicked: Function,
        onPlayClicked: Function,
        onFastForwardClicked: Function,
    },
    emits: ['skipTo']
};
</script>

<style lang="scss">
.controlBar {
    width: calc(100vw - 4em);
    height: 3em;

    padding: 1em 2em;

    background-color: #ffc0c0;

    display: flex;
    align-content: center;
    justify-content: space-evenly;

    .songInfo {
        height: 3em;
        scale: 120%;
    }

    .progressSlider {
        width: 40%;
    }
}
</style>