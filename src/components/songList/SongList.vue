<template>
    <div class="songList">
        <form class="songCreation" @submit="addSong">
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
            name: 'マヨイガ',
            artist: '羊文学',
            youtubeLink: 'https://www.youtube.com/watch?v=W4TOXpA7ktI',
        }
    },
    props: {
        songs: Array,
        onSongSelected: Function,
        onSongsShouldUpdate: Function,
    },
    methods: {
        async addSong(event) {
            event.preventDefault();

            const searchParams = new URLSearchParams({
                name: this.artist,
            });

            const searchResponse = await fetch(`${process.env.VUE_APP_BACKEND_URL}/artists/search?${searchParams}`, {
                mode: 'cors',
            });

            const searchJson = await searchResponse.json();

            let artistId;
            if (searchJson.exists) {
                artistId = searchJson.id;
            } else {
                const response = await fetch(`${process.env.VUE_APP_BACKEND_URL}/artists`, {
                    method: 'POST',
                    mode: 'cors',
                    headers: {
                        'Accept': 'application/json',
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        name: this.artist,
                    }),
                });

                artistId = (await response.json()).id;
            }

            await fetch(`${process.env.VUE_APP_BACKEND_URL}/songs`, {
                method: 'POST',
                mode: 'cors',
                headers: {
                    'Accept': 'application/json',
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    name: this.name,
                    artist_id: artistId,
                    youtube_link: this.youtubeLink,
                }),
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

    input[type="text"] {
        width: 30%;
    }

    input[type="submit"] {
        width: 7.5%;
    }
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