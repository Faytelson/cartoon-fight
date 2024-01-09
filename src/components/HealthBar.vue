<template>
  <div class="health-bar">
    <div class="health-bar__score">{{ progress }}</div>
    <div class="health-bar__track">
      <div
        class="health-bar__progress"
        :class="classes"
        :style="`width: ${parseInt(getProgress)}%`"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HealthBar",
  props: {
    progress: {
      type: Number,
      default: 100,
    },
  },
  computed: {
    getProgress() {
      return this.progress;
    },
    classes() {
      let classes = [];
      if (this.getProgress < 10) {
        classes = ["health-bar__progress_red"];
      } else if (this.getProgress < 50) {
        classes = ["health-bar__progress_yellow"];
      }
      return classes;
    },
  },
};
</script>

<style lang="scss">
.health-bar {
  width: 100%;

  &__score {
    font-size: 28px;
    font-weight: 500;
    text-align: right;
    margin-bottom: 2px;
  } 

  &__track {
    width: 100%;
    height: 40px;
    background-color: $color-light-grey;
  }

  &__progress {
    width: 100%;
    height: 100%;
    background-color: $color-success;

    &_yellow {
      background-color: $color-warning;
    }

    &_red {
      background-color: $color-danger;
    }
  }
}

@media screen and (max-width: 767px) {
  .health-bar {
    width: 100%;
  
    &__score {
      font-size: 28px;
      font-weight: 500;
      text-align: right;
      margin-bottom: 2px;
    } 
  
    &__track {
      width: 100%;
      height: 30px;
      background-color: $color-light-grey;
    }
  
    &__progress {
      width: 100%;
      height: 100%;
      background-color: $color-success;
  
      &_yellow {
        background-color: $color-warning;
      }
  
      &_red {
        background-color: $color-danger;
      }
    }
  }
}

@media screen and (max-width: 575px) {
  .health-bar {
    width: 100%;
  
    &__score {
      font-size: 20px;
    } 
  
    &__track {
      width: 100%;
      height: 20px;
      background-color: $color-light-grey;
    }
  
    &__progress {
      width: 100%;
      height: 100%;
      background-color: $color-success;
  
      &_yellow {
        background-color: $color-warning;
      }
  
      &_red {
        background-color: $color-danger;
      }
    }
  }
}
</style>
