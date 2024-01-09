<template>
  <button
    class="button-action"
    @click="emitEvent"
    :class="classes"
    :disabled="button.disabled"
  >
    <span class="button-action__title">{{ button.title }}</span>
  </button>
</template>

<script>
export default {
  name: "ButtonAction",
  components: {},
  props: {
    button: Object,
  },
  data() {
    return {};
  },
  computed: {
    classes() {
      let classes = [];
      if (this.button.id === 1) {
        classes = ["button-action_white"];
      } else if (this.button.id === 2) {
        classes = ["button-action_red"];
      } else if (this.button.id === 3) {
        classes = ["button-action_blue"];
      } else if (this.button.id === 4) {
        classes = ["button-action_black"];
      } else if (this.button.id === 5) {
        classes = ["button-action_pink"];
      }
      return classes;
    },
  },
  methods: {
    emitEvent() {
      this.$emit("emitBtnClick", this.button.id);
    },
  },
};
</script>

<style lang="scss">
.button-action {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 16px;
  height: 54px;
  border: none;
  border-radius: 8px;
  box-shadow: 1px 0 20px 0 $color-grey;
  cursor: pointer;

  &:disabled {
    opacity: 0.5;
    pointer-events: none;
  }

  &_red {
    background-color: $color-danger;
    color: $color-white;
  }

  &_blue {
    background-color: $color-primary;
    color: $color-black;
  }

  &_black {
    background-color: $color-black;
    color: $color-white;
  }

  &_pink {
    background-color: $color-pink;
    box-shadow: (0 0 10px 0) $color-grey;
  }

  &__title {
    @include font($font-main, 20px, 700);
    text-transform: uppercase;
  }
}

@media screen and (max-width: 1023px) {
  .button-action {
    &__title {
      font-size: 16px;
    }
  }
}

@media screen and (max-width: 575px) {
  .button-action {
    width: 100%;
  }
}
</style>
