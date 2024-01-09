<template>
  <div class="log-panel">
    <div class="log-panel__line">
      <div class="log-panel__turn">Ход {{ line.counter + 1 }}</div>
      <div
        class="log-panel__enemy"
        v-if="line.enemyDamage">
        Враг получил <span>{{ line.enemyDamage }}</span> единиц урона. Так держать!
      </div>
      <div
        class="log-panel__player"
        v-if="line.playerDamage">
        Игрок получил <span>{{ line.playerDamage }}</span> единиц урона. Жаль!
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    line: {
      type: Object,
      default: null,
    },
  },
};
</script>

<style lang="scss">
.log-panel {
  &__turn {
    padding: 10px;
    @include font($font-main, 28px, 700);
    background-color: $color-light-grey-bg;
    color: $color-black;
    text-align: center;
  }

  &__enemy {
    padding: 10px;
    @include font($font-main, 18px, 500);
    background-color: $color-success-bg;
    color: $color-black;

    & span {
      font-weight: 900;
    }
  }

  &__player {
    padding: 10px;
    @include font($font-main, 18px, 500);
    background-color: $color-danger-bg;
    color: $color-black;

    & span {
      font-weight: 900;
    }
  }
}

@media screen and (max-width: 575px) {
  .log-panel {
    &__turn {
      font-size: 20px;
    }

    &__enemy,
    &__player {
      font-size: 14px;
    }
  }
}
</style>
