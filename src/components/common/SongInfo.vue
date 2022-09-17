<template>
    <div class="songInfo">
        <span class="songName">{{ songName }}</span>
        <span class="songArtist">{{ songArtist }}</span>
    </div>
</template>

<script>
export default {
    name: 'SongInfo',
    props: {
        songName: String,
        songArtist: String,
    },
};
</script>

<style scoped lang="scss">
.songInfo {
    width: 10em;
    height: 100%;

    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: left;

    .songName {
        font-weight: bolder;
    }

    .songArtist {
        font-size: 0.75em;
    }
}
</style>