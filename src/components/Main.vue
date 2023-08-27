<script lang="ts">
import { defineComponent } from 'vue';

interface MainComponentData {
  monsterHealth: number;
  yourHealth: number;
  log: string[];
  specialAttack: boolean;
}

export default defineComponent({
  data(): MainComponentData {
    return {
      monsterHealth: 100,
      yourHealth: 100,
      log: [],
      specialAttack: false,
    };
  },
  methods: {
    attack() {
      let yourDamage = Math.floor(Math.random() * (15 - 5 + 1) + 5);
      if (this.specialAttack) {
        yourDamage += Math.floor(Math.random() * (5 - -5 + 1) - 5);
        this.specialAttack = false;
      }

      let monsterDamage = Math.floor(Math.random() * (15 - 5 + 1) + 5);
      this.monsterHealth -= yourDamage;
      this.yourHealth -= monsterDamage;
      this.logger(yourDamage, 'Player');
      this.logger(monsterDamage, 'Monster');

      if (this.monsterHealth <= 0 && this.yourHealth <= 0) {
        this.monsterHealth = 0;
        this.yourHealth = 0;
        setTimeout(() => {
          this.draw();
          this.yourHealth = 100;
          this.monsterHealth = 100;
          this.clearLog();
        }, 0);

        return;
      }
      if (this.monsterHealth <= 0) {
        this.monsterHealth = 0;
        setTimeout(() => {
          this.win();
          this.yourHealth = 100;
          this.monsterHealth = 100;
          this.clearLog();
        }, 0);
        return;
      }

      if (this.yourHealth <= 0) {
        this.yourHealth = 0;
        setTimeout(() => {
          this.lose();
          this.yourHealth = 100;
          this.monsterHealth = 100;
          this.clearLog();
        }, 0);

        return;
      }
    },
    special() {
      this.specialAttack = true;
    },
    heal() {
      this.yourHealth += 5;
      if (this.yourHealth > 100) {
        this.yourHealth = 100;
      }
      this.monsterHealth += 2;
      if (this.monsterHealth > 100) {
        this.monsterHealth = 100;
      }
    },
    win() {
      alert('You won!');
    },
    lose() {
      alert('You lost');
    },
    draw() {
      alert('You both died!');
    },
    surrender() {
      this.yourHealth = 0;

      setTimeout(() => {
        alert('You lost');
        this.yourHealth = 100;
        this.monsterHealth = 100;
        this.clearLog();
      }, 0);
    },
    logger(damage: number, dealer: string) {
      this.log.unshift(`${dealer} attacks and deals ${damage} damage`);
    },
    words(string: string) {
      return string.split(/\s+/);
    },
    clearLog() {
      this.log = [];
    },
  },
  mounted() {
    console.log(this.monsterHealth);
  },
});
</script>

<template>
  <div class="wrapper">
    <div class="health">
      <div class="health__header">Monster health</div>
      <div
        v-bind:style="{
          backgroundColor: 'seagreen',
          width: monsterHealth + '%',
        }"
        class="health__bar"
      ></div>
    </div>
    <div class="health">
      <div class="health__header">Your health</div>
      <div
        v-bind:style="{
          backgroundColor: 'seagreen',
          width: `${yourHealth}%`,
        }"
        class="health__bar"
      ></div>
    </div>
    <div class="buttons">
      <button class="button" v-on:click="attack">Attack</button>
      <button
        v-bind:class="{ special: specialAttack, button: true }"
        v-on:click="special"
      >
        Special Attack
      </button>
      <button class="button" v-on:click="heal">Heal</button>
      <button class="button" v-on:click="surrender">Surrender</button>
    </div>
    <div class="log">
      <div class="log__header">Battle Log</div>
      <div v-for="attack in log">
        <span v-for="(word, index) in words(attack)" :key="index">
          <span
            v-if="index === 0 && word === 'Monster'"
            class="firstWordMonster"
            >{{ word }}&nbsp;</span
          >
          <span
            v-else-if="index === 0 && word === 'Player'"
            class="firstWordPlayer"
            >{{ word }}&nbsp;</span
          >
          <span v-else>{{ word }}&nbsp;</span>
        </span>
      </div>
    </div>
  </div>
</template>
<style>
.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
  margin-top: 10px;
}
.health {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 15px;
  height: 150px;
  width: 60%;
  padding: 0 10px;
  border-radius: 5px;
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.615);
}

.health__header {
  font-size: 1.5rem;
  font-weight: 700;
}

.health__bar {
  height: 50px;
  align-self: flex-start;
}

.button {
  border: none;
  border-radius: 10px;
  text-transform: uppercase;
  background-color: rgb(143, 33, 82);
  height: 60px;
  width: 200px;
  color: white;
  font-size: 1.2rem;
  cursor: pointer;
  transition: all 0.2s;
}

.button:hover {
  background-color: rgba(143, 33, 82, 0.8);
}

.buttons {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  width: 500px;
}

.special {
  background-color: rgba(186, 187, 196, 0.698);
  color: black;
}

.special:hover {
  background-color: rgba(186, 187, 196, 0.698);
}

.log {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  gap: 15px;
  min-height: 100px;
  width: 60%;
  border-radius: 5px;
  box-shadow: 0px 0px 5px 0px rgba(0, 0, 0, 0.615);
  margin-bottom: 50px;
}

.log__header {
  font-size: 1.5rem;
  font-weight: 700;
}

.firstWordMonster {
  color: orange;
  font-weight: 700;
}

.firstWordPlayer {
  color: violet;
  font-weight: 700;
}
</style>
