<template>
    <div :class="{ songListEntry: true, darkened: this.darkened }">
        <SongInfo :song-name="song.name" :song-artist="song.artist" />
        <SongLength :seconds="song.duration" />
    </div>
</template>

<script>
import SongInfo from '@/components/common/SongInfo';
import SongLength from '@/components/common/SongLength';
export default {
    name: 'SongListEntry',
    components: { SongLength, SongInfo },
    props: {
        song: Object,
        darkened: Boolean
    }
};
</script>

<style scoped lang="scss">
.songListEntry {
    padding: 0 1.5em;
    height: 3em;

    display: flex;
    justify-content: space-between;

    border-radius: 0.5em;
}

.darkened {
    background-color: #ffd7d7;
}
</style>