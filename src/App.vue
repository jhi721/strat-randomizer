<template>
  <div id="app">
    <button v-if="status === 'Waiting'" class="strat-button" @click="randomizeStrat">
      Get Pro Strat
    </button>
    <strat v-else :strat="currentStrat" :status="status"/>
    <div
      :class="[`reset-button`, { show: status === 'Completed' }]" @click="resetState">
      <img src="./assets/refresh-icon.svg" alt="">
    </div>
  </div>
</template>

<script>
import Strat from '@/components/strat.vue';

export default {
  name: 'App',
  components: { Strat },
  data() {
    return {
      strats: [
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
      ],
      currentStrat: '',
      status: 'Waiting',
    };
  },
  methods: {
    randomizeStrat() {
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
    },
    resetState() {
      this.currentStrat = '';
      this.status = 'Waiting';
    },
    getRandomIndex() {
      return Math.floor(Math.random() * (this.strats.length));
    },
  },
};
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

  background: url("./assets/background.jpg") center no-repeat;
}

.strat-button {
  position: relative;
  border: 2px solid #000;
  background: #56575C;
  font-size: 40px;
  overflow: hidden;

  color: inherit;
  cursor: pointer;

  padding: 10px 20px;
  transition: transform .2s;

  animation: appear 1s forwards;

  &:active {
    transform: scale(0.9) translateY(10px);
  }

  &::after, &::before {
    position: absolute;
    content: '';
    height: 50%;
    left: 0;
    right: 0;

    background: #000;
    mix-blend-mode: overlay;
    transition: transform .5s;
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
  transition: transform .5s ease-in-out;

  &:hover, &:active {
    transform: translateY(65px) rotate(420deg) !important;
  }

  &.show {
    transform: translateY(65px) rotate(0);
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
