<template>
  <v-layout row align-center shrink>
    <v-btn icon class="mr-1" @click.stop="muteVolume">
      <v-icon medium :style="primaryStyle">{{
        isMuted ? 'volume_off' : 'volume_up'
      }}</v-icon>
    </v-btn>

    <div v-if="width" :style="{ width: width + 'px' }">
      <v-slider
        min="0"
        max="1"
        step="0.1"
        v-model="volumeControl"
        :color="primaryColor"
      >
      </v-slider>
    </div>
    <v-slider
      v-else
      min="0"
      max="1"
      step="0.1"
      v-model="volumeControl"
      :color="primaryColor"
    >
    </v-slider>
  </v-layout>
</template>

<script lang="ts">
import { Component, Mixins, Prop } from 'vue-property-decorator';
import { Action, State } from 'vuex-class';

import PlayerBarColorMixin from '@/mixins/PlayerBarColorMixin';
import { CHANGE_VOLUME, MUTE_VOLUME } from '@/store/actions.type';

@Component
export default class PlayerVolume extends Mixins(PlayerBarColorMixin) {
  @Prop() width!: number;

  @State(state => state.musicPlayer.volume) volume!: number;
  @State(state => state.musicPlayer.isMuted) isMuted!: boolean;

  @Action [CHANGE_VOLUME]: (volume: number) => void;
  @Action [MUTE_VOLUME]: () => void;

  get volumeControl(): number {
    return this.isMuted ? 0 : this.volume;
  }

  set volumeControl(value: number) {
    this.changeVolume(value);
  }
}
</script>
