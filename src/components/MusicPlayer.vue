<template>
    <SongList
        :songs="songs"
        :on-song-selected="i => currentSong = i"
        @songsShouldUpdate="updateSongs" />
    <ControlBar
        :song="songs[currentSong]"
        :current-time="currentTime"
        @rewindClicked="rewindClicked"
        @playClicked="playClicked"
        @fastForwardClicked="fastForwardClicked"
        @skipTo="skipTo"
        :volume="volume"
        @volumeChanged="vol => this.volume = vol"
        :is-playing="isPlaying" />
    <audio ref="audio"></audio>
</template>

<script>
import ControlBar from '@/components/playbackControl/ControlBar';
import SongList from '@/components/songList/SongList';
import { orderBy } from 'lodash';

export default {
    name: 'MusicPlayer',
    components: { SongList, ControlBar },
    data() {
        return {
            songs: [],
            currentSong: -1,
            currentTime: 0,
            volume: 50.0,
            isPlaying: false,
            pageJustLoaded: true,
        }
    },
    methods: {
        rewindClicked() {
            if (this.currentTime < 1.0 || this.currentSong > 0) {
                this.currentSong--;
            } else {
                this.skipTo(0.0);
            }
        },
        playClicked() {
            if (this.isPlaying) {
                this.$refs.audio.pause();
            } else {
                this.$refs.audio.play();
            }

            this.isPlaying = !this.isPlaying;
        },
        fastForwardClicked() {
            if (this.currentSong + 1 === this.songs.length) {
                this.currentSong = 0;
                this.pageJustLoaded = true;
            } else {
                this.currentSong++;
            }
        },
        skipTo(time) {
            this.$refs.audio.fastSeek(time);
        },
        async updateSongs() {
            const response = await fetch(`${process.env.BACKEND_URL}/songs`, {
                mode: 'cors',
            });

            this.songs = orderBy(await response.json(), 'id');
        },
    },
    async mounted() {
        await this.updateSongs();

        this.currentSong = 0;

        this.$refs.audio.addEventListener('timeupdate', () => {
            this.currentTime = this.$refs.audio.currentTime;
        });

        this.$refs.audio.addEventListener('ended', () => {
            this.fastForwardClicked();
        });
    },
    watch: {
        currentSong(newSong) {
            const song = this.songs[newSong];

            if (song === undefined) return;

            this.$refs.audio.src = `${process.env.BACKEND_URL}/songs/${song.id}/stream`;
            this.$refs.audio.volume = this.volume / 100.0;

            if (this.pageJustLoaded) {
                this.pageJustLoaded = false;
                this.isPlaying = false;

                return;
            }

            this.$refs.audio.play();

            this.isPlaying = true;
        },
        volume(newVolume) {
            this.$refs.audio.volume = newVolume / 100.0;
        },
    },
};
</script>

<style lang="scss">

</style>