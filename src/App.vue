<template>
  <div id="app">
    <button
      v-if="status === 'Waiting'"
      class="strat-button"
      @click="randomizeStrat"
    >
      Get Pro Strat
    </button>
    <Strat v-else :strat="currentStrat" :status="status" />

    <img
      :class="[`reset-button`, { show: status === 'Completed' }]"
      @click="resetState"
      src="./assets/refresh-icon.svg"
      alt=""
    />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import Strat from '@/components/Strat.vue';

@Component({
  components: {
    Strat,
  },
})
export default class App extends Vue {
  private strats: Array<string> = [
    'Pistols Only',
    'SMG Only',
    'DMR Only',
    'Shotgun Only',
    'Shields Only',
    'LMG Only',
    'Hipfire Only',
    'Dropshot Only',
    'Prone(Def Only)',
    'Crouch Only',
    'Burst Mode',
    'One-Tap Mode',
    'Sniper Rifle',
    'One-Bullet Mag',
    'Teamkill',
    'Suicide',
  ];
  private currentStrat = '';
  private status: 'Waiting' | 'Randomizing' | 'Completed' = 'Waiting';

  public randomizeStrat(): void {
    if (this.status === 'Waiting' || this.status === 'Completed') {
      setTimeout(() => {
        this.currentStrat = '';
        this.status = 'Randomizing';
        const interval = setInterval(() => {
          this.currentStrat = this.strats[this.getRandomIndex()];
        }, 150);
        setTimeout(() => {
          this.status = 'Completed';
          clearInterval(interval);
        }, 5000);
      }, 500);
    }
  }

  public resetState(): void {
    this.currentStrat = '';
    this.status = 'Waiting';
  }

  public getRandomIndex(): number {
    return Math.floor(Math.random() * this.strats.length);
  }
}
</script>

<style lang="scss">
* {
  font-family: 'Recursive', sans-serif;
  color: #fff;
  box-sizing: border-box;
}

body {
  padding: 0;
  margin: 0;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background-image: url('./assets/test.jpg');
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.strat-button {
  position: relative;
  border: 2px solid #000;
  background: #56575c;
  font-size: 40px;
  overflow: hidden;
  color: inherit;
  cursor: pointer;
  padding: 10px 20px;
  transition: transform 0.2s;
  animation: appear 1s forwards;
  &:active {
    transform: scale(0.9) translateY(10px);
  }
  &::after,
  &::before {
    position: absolute;
    content: '';
    height: 50%;
    left: 0;
    right: 0;
    background: #000;
    mix-blend-mode: overlay;
    transition: transform 0.5s;
  }
  &::before {
    top: -50%;
  }
  &::after {
    top: 100%;
  }
  &:hover {
    &::before {
      transform: translateY(100%);
    }
    &::after {
      transform: translateY(-100%);
    }
  }
}

.reset-button {
  position: absolute;
  width: 40px;
  height: 40px;
  top: -50px;
  border-radius: 100%;
  padding: 8px;
  cursor: pointer;
  background: #fff;
  transition: transform 0.5s ease-in-out;

  &.show {
    transform: translateY(65px) rotate(420deg);
  }
}

@keyframes appear {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
