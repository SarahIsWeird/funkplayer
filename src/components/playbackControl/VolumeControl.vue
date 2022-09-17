<template>
    <div class="volumeControl">
        <span @click="mute" class="material-symbols-outlined">{{ getVolumeSymbol() }}</span>
        <input
            class="volumeSlider"
            type="range"
            min="0"
            max="100"
            step="0.1"
            :value="volume"
            @input="setVolume($event.target.value)" />
    </div>
</template>

<script>
export default {
    name: 'VolumeControl',
    props: {
        volume: Number,
        onVolumeChanged: Function,
    },
    data() {
        return {
            unmutedVolume: 50.0,
            explicitlyMuted: false,
        }
    },
    methods: {
        mute() {
            if (this.explicitlyMuted) {
                this.setVolume(this.unmutedVolume)
            } else {
                this.unmutedVolume = this.volume
                this.setVolume(0)
            }

            this.explicitlyMuted = !this.explicitlyMuted
        },
        getVolumeSymbol() {
            // noinspection EqualityComparisonWithCoercionJS
            if (this.volume == 0) {
                return "volume_off"
            }

            if (this.volume <= 10.0) {
                return "volume_mute"
            }

            if (this.volume <= 33.3) {
                return "volume_down"
            }

            return "volume_up"
        },
        setVolume(newVolume) {
            this.$emit('volumeChanged', Number.parseFloat(newVolume));
        }
    },
    watch: {
        volume(newVolume) {
            if (newVolume > 0.0) {
                this.explicitlyMuted = false

                this.unmutedVolume = newVolume
            }
        }
    }
};
</script>

<style lang="scss">
.volumeControl {
    width: 15%;
    height: 3em;

    display: flex;

    span {
        line-height: 2em;
        vertical-align: middle;

        padding-right: 0.5em;

        cursor: pointer;
    }

    .volumeSlider {
        width: 100%;
    }
}
</style>