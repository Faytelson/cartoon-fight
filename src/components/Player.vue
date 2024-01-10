<template>
  <div class="player">
    <div class="player__title">{{ player.avatarDescription }}</div>
    <div class="player__avatar">
      <img
        :src="require(`@/assets/images/${player.avatarSrc}`)"
        :alt="player.avatarDescription"
        :title="player.avatarDescription"
        class="player__avatar-img" />
      <div
        class="player__avatar-cover"
        v-if="getAnimation">
        <div
          v-if="player.id === 'enemy'"
          class="player__avatar-cover-container">
          <img
            :src="require('@/assets/images/blood.png')"
            alt="Кровь"
            class="player__avatar-cover-img" />
        </div>
        <div
          v-else
          class="player__avatar-cover-container">
          <img
            :src="require('@/assets/images/blood.png')"
            alt="Кровь"
            class="player__avatar-cover-img player__avatar-cover-img_player" />
        </div>
      </div>
      <div
        class="player__avatar-cover"
        v-if="getSpeech">
        <div
          v-if="player.id === 'enemy'"
          class="player__avatar-cover-container">
          <span class="player__avatar-cover-description">{{ getCurrentMessage }}</span>
        </div>
        <div
          v-else
          class="player__avatar-cover-container">
          <span class="player__avatar-cover-description">{{ getCurrentMessage }}</span>
        </div>
      </div>
    </div>
    <div class="player__health-bar">
      <HealthBar :progress="getProgress"></HealthBar>
    </div>
  </div>
</template>

<script>
import HealthBar from "./HealthBar.vue";

export default {
  name: "Player",
  components: {
    HealthBar,
  },
  props: {
    player: {
      type: Object,
    },
    counter: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    getProgress() {
      return this.player.progress;
    },
    getAnimation() {
      return this.player.animation;
    },
    getCounter() {
      return this.counter;
    },
    getCurrentMessage() {
      return this.player.messages[this.getCounter];
    },
    getSpeech() {
      return this.player.speech;
    },
  },
};
</script>

<style lang="scss" scoped>
.player {
  &__title {
    @include font($font-main, 50px, 700);
    margin-bottom: 30px;
    text-align: center;
  }

  &__avatar {
    width: 350px;
    height: 200px;
    margin-bottom: 15px;
    position: relative;
  }

  &__avatar-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center 10%;
  }

  &__avatar-cover {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: $color-white;
    animation: pulse 4s linear infinite;
  }

  &__avatar-cover-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  &__avatar-cover-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center top;
    opacity: 0.5;

    &_player {
      transform: scale(-1, 1);
    }
  }

  &__avatar-cover-description {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 24px;
    font-weight: 900;
    font-family: $font-main;
    text-align: center;
    color: $color-black;
    width: max-content;
    max-width: 100%;
    padding: 12px;
  }
}

@keyframes pulse {
  0% {
    opacity: 0;
  }
  30% {
    opacity: 0.8;
  }
  100% {
    opacity: 0.8;
  }
}

@media screen and (max-width: 1023px) {
  .player {
    &__avatar {
      width: 300px;
    }

    &__avatar-cover-description {
      font-size: 20px;
    }
  }
}

@media screen and (max-width: 767px) {
  .player {
    &__title {
      font-size: 40px;
    }

    &__avatar {
      width: 240px;
    }

    &__avatar-cover-description {
      font-size: 16px;
    }
  }
}

@media screen and (max-width: 575px) {
  .player {
    &__title {
      font-size: 24px;
      margin-bottom: 10px;
    }

    &__avatar {
      width: 100%;
      height: 140px;
      margin-bottom: 0;
    }

    &__avatar-cover-description {
      font-size: 16px;
    }
  }
}
</style>
