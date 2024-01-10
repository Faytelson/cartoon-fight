<template>
  <div id="app" class="app">
    <div class="app__container">
      <div class="app__players">
        <Player v-for="player in players" :key="player.id" :player="player" :counter="counter"> </Player>
      </div>
      <div class="app__actions-panel">
        <div class="app__buttons-container">
          <div class="app__start-button" v-if="!isGameOn">
            <ButtonAction :button="buttonAction" @emitBtnClick="handleClick"> </ButtonAction>
          </div>
          <div class="app__back-button" v-if="isBack">
            <ButtonAction :button="buttonBack" @emitBtnClick="handleClick"></ButtonAction>
          </div>
          <div class="app__buttons" v-if="isGameOn && !isBack">
            <div class="app__button" v-for="button in buttons" :key="button.id">
              <ButtonAction :button="button" @emitBtnClick="handleClick"> </ButtonAction>
            </div>
          </div>
        </div>
      </div>
      <div class="app__log-panel" v-for="(line, index) in logLines" :key="index">
        <LogPanel :line="line"></LogPanel>
      </div>
      <div class="app__dialog" v-if="isDialog">
        <Dialog :info="dialogInfo"></Dialog>
      </div>
    </div>
  </div>
</template>

<script>
import Player from "./components/Player.vue";
import ButtonAction from "./components/ButtonAction.vue";
import Dialog from "./components/Dialog.vue";
import LogPanel from "./components/LogPanel.vue";

export default {
  name: "App",
  components: {
    Player,
    ButtonAction,
    Dialog,
    LogPanel,
  },
  data() {
    return {
      players: {
        enemy: {
          id: "enemy",
          avatarSrc: "hare.webp",
          avatarDescription: "Заяц",
          progress: 100,
          damage: 0,
          animation: false,
          speech: false,
          messages: [
            "Расскажи, Снегурочка, где была? Расскажи-ка, милая, как дела?",
            "А ну-ка, давай-ка, плясать выходи! А?",
            "Аста ла виста, baby!",
            "I'll be back",
            "Киборги не чувствуют боли. Я чувствую",
            "Ждёт моих подарочков ребятня! И тебе достанется от меня!",
            "А ну-ка, давай-ка, плясать выходи! А?",
            "Ха-ха-ха!",
            "Я — шоколадный заяц, я — лясковый мерзавец, я — сладкий на все сто, о-о-о…",
            "А нам всё равно, а нам всё равно, хоть боимся мы волка и сову",
            "Меня нельзя подкупить. Со мной не договориться. Мне не ведомы жалость, раскаяние или страх",
            "В теле человека 215 костей, я сломал всего одну",
          ],
        },
        player: {
          id: "player",
          avatarSrc: "wolf.webp",
          avatarDescription: "Волк",
          progress: 100,
          damage: 0,
          animation: false,
          speech: false,
          messages: [
            "Ну, Заяц! Ну, Абдурахман! Ну, погоди-и-и-и!",
            "За тобою бегала, Дед Мороз. Пролила немало я горьких слёз!",
            "Нет, Дед Мороз! Нет, Дед Мороз! Нет, Дед Мороз, погоди!",
            "Потише, железный мозг!",
            "Где твой самообучающийся процессор?",
            "И наконец, сбываются все мечты. Лучший мой подарочек — это ты!",
            "Нет, Дед Мороз, нет, Дед Мороз, нет, Дед Мороз, погоди!",
            "Ну, погоди! Ещё — не вечер!",
            "Ай! Ну, Дед Мороз, ну, Дед Мороз, ну, Дед Мороз, погоди-и-и-и!",
            "Ну, Заяц...",
            "И верю я — наступит встреча под крышей дома твоего!",
            "Ну, шкелет, погоди!",
          ],
        },
      },
      buttons: [
        {
          title: "Атаковать",
          id: 1,
          disabled: false,
        },
        {
          title: "Усиленная атака",
          id: 2,
          disabled: true,
        },
        {
          title: "Подлечиться",
          id: 3,
          disabled: false,
        },
        {
          title: "Сдаться врагу",
          id: 4,
          disabled: false,
        },
      ],
      buttonAction: {
        title: "Начать игру",
        id: 5,
        disabled: false,
      },
      buttonBack: {
        title: "В начало",
        id: 6,
        disabled: false,
      },
      dialogInfo: {
        messageType: null,
        side: null,
        name: null,
        damage: null,
      },
      counter: 0,
      specialAttackCounter: 0,
      isGameOn: false,
      isDialog: false,
      healthIncreased: false,
      isBack: false,
      looser: null,
      logLines: [],
    };
  },
  methods: {
    handleClick(id) {
      if (id === 1) {
        this.addLogLine();
        this.attack("enemy");
      } else if (id === 2) {
        this.addLogLine();
        this.attack("enemy", true);
        this.specialAttackCounter = -1;
      } else if (id === 3) {
        this.changeHealthProgress("player", false, true);
        this.healthIncreased = true;
        this.handleButton(this.buttons[2], "disable");
      } else if (id === 4) {
        this.announceGameOver("player");
      } else if (id === 5) {
        this.isGameOn = true;
        this.disableAllButtons();
        this.handleButton(this.buttons[0]);
        this.showDialog("start");
      } else if (id === 6) {
        this.reset();
      }
    },
    setDelay(time) {
      return new Promise((res) => {
        setTimeout(() => {
          res();
        }, time);
      });
    },
    attack(side, isSpecial) {
      this.disableAllButtons();
      this.showDialog("turn", side);
      this.setDelay(2000)
        .then(() => {
          this.hideDialog();
          this.animateBattle(side, true);
          return this.setDelay(4000);
        })
        .then(() => {
          this.animateBattle(side, false);
          this.changeHealthProgress(side, isSpecial);
          return this.setDelay(1000);
        })
        .then(() => {
          this.showDialog("damage", side);
          return this.setDelay(2000);
        })
        .then(() => {
          this.hideDialog();
          this.checkNextTurn(side);
        });
    },
    handleButton(btn, status) {
      if (status === "disable") {
        btn.disabled = true;
      } else {
        btn.disabled = false;
      }
    },
    disableAllButtons() {
      this.buttons.forEach((btn) => {
        this.handleButton(btn, "disable");
      });
    },
    showDialog(type, side) {
      if (side) {
        this.dialogInfo.side = side;
        this.dialogInfo.name = this.players[side].avatarDescription;
        this.dialogInfo.damage = this.players[side].damage;
      }
      this.isDialog = true;
      this.dialogInfo.messageType = type;
    },
    hideDialog() {
      this.isDialog = false;
      this.dialogInfo = {
        messageType: null,
        side: null,
        name: null,
        damage: null,
      };
    },
    throwRandomNum() {
      let random = Math.ceil(Math.random() * 10);
      return random + 8;
    },
    calculateProgress(side) {
      let currentPlayer = this.players[side];
      currentPlayer.progress = Number(currentPlayer.progress) - Number(currentPlayer.damage);
      if (currentPlayer.progress <= 0) {
        currentPlayer.progress = 0;
        this.looser = currentPlayer.id;
      } else if (this.progress >= 100) {
        currentPlayer.progress = 100;
      }
    },
    changeHealthProgress(side, special, inc) {
      if (inc) {
        this.players[side].damage = -this.throwRandomNum();
        this.calculateProgress(side);
      } else {
        if (special) {
          this.players[side].damage = this.throwRandomNum() + 5;
        } else {
          this.players[side].damage = this.throwRandomNum();
        }
        this.calculateProgress(side);
        if (side === "enemy") {
          this.logLines[this.counter].enemyDamage = this.players[side].damage;
        } else if (side === "player") {
          this.logLines[this.counter].playerDamage = this.players[side].damage;
        }
      }
    },
    animateBattle(side, state) {
      this.players[side].animation = state;
      for (let key in this.players) {
        if (key !== side) {
          this.players[key].speech = state;
        }
      }
    },
    checkNextTurn(side) {
      if (!this.looser) {
        if (side === "enemy") {
          this.attack("player");
        } else {
          this.counter++;
          this.specialAttackCounter++;
          this.handleButton(this.buttons[0]);
          this.handleButton(this.buttons[3]);
          if (!this.healthIncreased) {
            this.handleButton(this.buttons[2]);
          }
          this.buttons[1].disabled = this.specialAttackCounter < 5;
        }
      } else {
        this.announceGameOver(this.looser);
      }
    },
    addLogLine() {
      let newLine = {
        counter: this.counter,
        enemyDamage: null,
        playerDamage: null,
      };
      this.logLines.push(newLine);
    },
    announceGameOver(id) {
      if (id === "enemy") {
        this.showDialog("end success");
      } else {
        this.showDialog("end fail");
      }
      this.isBack = true;
    },
    reset() {
      this.hideDialog();
      this.counter = 0;
      this.specialAttackCounter = 0;
      this.isGameOn = false;
      this.healthIncreased = false;
      this.isBack = false;
      for (let key in this.players) {
        this.players[key].progress = 100;
        this.players[key].damage = 0;
      }
      this.looser = null;
      this.logLines = [];
    },
  },
};
</script>

<style lang="scss">
.app {
  &__container {
    max-width: 1000px;
    margin-right: auto;
    margin-left: auto;
    margin-top: 10%;
    position: relative;
  }

  &__players {
    display: flex;
    justify-content: space-around;
    margin-bottom: 40px;
  }

  &__actions-panel {
    margin-bottom: 40px;
  }

  &__buttons {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__start-button,
  &__back-button {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  &__dialog {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 397px;
    z-index: 1;
  }
}

@media screen and (max-width: 1023px) {
  .app {
    &__container {
      max-width: 700px;
    }

    &__buttons {
      gap: 10px;
    }
  }
}

@media screen and (max-width: 767px) {
  .app {
    &__container {
      max-width: 500px;
    }

    &__players {
      justify-content: space-between;
    }
  }
}

@media screen and (max-width: 575px) {
  .app {
    &__container {
      max-width: 100%;
      padding-left: 16px;
      padding-right: 16px;
      margin-top: 0;
    }

    &__players {
      flex-direction: column;
      gap: 15px;
    }

    &__dialog {
      height: 483px;
    }

    &__buttons {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
    }
  }
}
</style>
