<template>
    <div class="songList">
        <form class="songCreation">
            <input type="text" placeholder="Name" v-model="name" />
            <input type="text" placeholder="Artist" v-model="artist" />
            <input type="text" placeholder="YouTube link" v-model="youtubeLink" />
            <input type="submit" value="Add song" />
        </form>
        <ul>
            <li v-for="(song, i) in songs" :key="song.id">
                <SongListEntry @click="onSongSelected(i)" :song="song" :darkened="i % 2 === 0" />
            </li>
        </ul>
    </div>
</template>

<script>
import SongListEntry from '@/components/songList/SongListEntry';

export default {
    name: 'SongList',
    components: { SongListEntry },
    data() {
        return {
            name: '',
            artist: '',
            youtubeLink: '',
        }
    },
    props: {
        songs: Array,
        onSongSelected: Function,
        onSongsShouldUpdate: Function,
    },
    methods: {
        async addSong() {
            const searchParams = new URLSearchParams({
                name: this.artist,
            });

            const searchResponse = await fetch('http://localhost:8081/artists/search?' + searchParams, {
                mode: 'cors',
            });

            const searchJson = await searchResponse.json();

            let artistId;
            if (searchJson.exists) {
                artistId = searchJson.id;
            } else {
                artistId = await fetch('http://localhost:8081/artists', {
                    method: 'POST',
                    mode: 'cors',
                    body: {
                        name: this.artist,
                    },
                });
            }

            await fetch('http://localhost:8081/songs', {
                method: 'POST',
                mode: 'cors',
                body: {
                    name: this.name,
                    artist_id: artistId,
                    youtube_link: this.youtubeLink,
                },
            });

            this.onSongsShouldUpdate();
        }
    }
};
</script>

<style lang="scss">
.songCreation {
    height: 2em;

    padding: 0.5em 1.5em;

    display: flex;
    justify-content: space-between;

    border-radius: 0.5em;
}

.songList {
    width: calc(100vw - 4em);
    height: calc(100vh - 9em);

    padding: 2em;

    background-color: #ffe0e0;

    overflow: scroll;

    li {
        list-style: none;
    }
}
</style>