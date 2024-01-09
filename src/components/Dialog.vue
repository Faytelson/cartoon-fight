<template>
  <div class="dialog">
    <div class="dialog__info">
      <div v-if="info.messageType === 'start'">
        <p>Твоя задача - победить злого Зайца.</p>
        <p>Атакуй врага каждый свой ход.</p>
        <p>Атаковать усиленным ударом можно после каждых 5 обычных ударов</p>
        <p>Поднять свое здоровье можно только один раз за всю игру, так что не увлекайся.</p>
        <p>Сдаться можно почти всегда, но не сдавайся, друг!</p>
        <p>В общем, поехали! Жми АТАКОВАТЬ</p>
      </div>
      <p v-else-if="info.messageType === 'turn' && getSide === 'player'">Держит удар {{ getName | capitalizeName }}</p>
      <p v-else-if="info.messageType === 'turn' && getSide === 'enemy'">Получает {{ getName | capitalizeName }}</p>
      <p v-else-if="info.messageType === 'damage' && getSide === 'player'">{{ getName | capitalizeName }} получил {{ getDamage }} ед. урона</p>
      <p v-else-if="info.messageType === 'damage' && getSide === 'enemy'">{{ getName | capitalizeName }} отхватил {{ getDamage }} ед. урона</p>
      <p v-else-if="info.messageType === 'end success'">
        <span>Ну и ну! Вот это победа!</span>
        <embed
          :src="require('@/assets/media/cricket-sound.mp3')"
          width="70"
          height="45"
          align="left"
          hidden
          autostart
          loop="True" />
      </p>
      <p v-else-if="info.messageType === 'end fail'">
        <span>Тебя победил Заяц</span>
        <video
          autoplay
          preload="metadata"
          class="dialog__video">
          <source
            :src="require('@/assets/media/foo.mp4')"
            type="video/mp4" />
        </video>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    info: Object,
  },
  computed: {
    getSide() {
      return this.info.side;
    },
    getName() {
      return this.info.name;
    },
    getDamage() {
      return this.info.damage;
    },
  },
  filters: {
    capitalizeName(v) {
      return v[0].toUpperCase() + v.slice(1);
    },
  },
};
</script>

<style lang="scss">
.dialog {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: $color-white;

  &__info {
    font-family: $font-main;
    font-size: 20px;
    color: $color-black;
    max-width: 600px;
    text-align: center;
  }

  &__video {
    width: 100%;
    height: auto;
  }
}

@media screen and (max-width: 575px) {
  .dialog {
    padding: 16px;

    &__info {
      font-size: 16px;
      max-width: 100%;
    }
  }
}
</style>
