<template>
  <div class="strat-wrapper">
    <div :class="[`pro-strat`, {'strat-selected': status === 'Completed'}]">
      {{ strat }}
      <span class="border border-left"></span>
      <span class="border border-top"></span>
      <span class="border border-right"></span>
      <span class="border border-bottom"></span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    strat: {
      type: String,
      required: true,
    },
    status: {
      type: String,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.strat-wrapper {
  animation: show-strat 5s forwards;
  opacity: .3;
  transform: scale(.7);

  display: flex;
  flex-direction: column;
}

.pro-strat {
  font-size: 40px;
  transition: all 1s;
  position: relative;
  padding: 10px 20px;
  overflow: hidden;
  color: #fff;
  background: #56575c;

  &::after, &::before {
    position: absolute;
    content: '';
    top: 0;
    bottom: 0;

    mix-blend-mode: overlay;
    transition: transform 1s 1.5s;
    width: 50%;
    background: #000;
  }

  &::before {
    left: -50%;
  }

  &::after {
    right: -50%;
  }
}

.strat-selected {
  transform: scale(1.2);

  &::after {
    transform: translateX(-101%);
  }

  &::before {
    transform: translateX(101%);
  }

  & .border {
    &-left {
      transform: translateY(-100%);
    }

    &-top{
      transform: translateX(100%);
    }

    &-right {
      transform: translateY(100%);
    }

    &-bottom {
      transform: translateX(-100%);
    }
  }
}

.border {
  position: absolute;
  content: '';
  background: #fff;
  z-index: -1;

  transition: transform 1s .5s;

  &-left {
    left: 0;
    bottom: -100%;
  }

  &-top {
    top: 0;
    left: -100%;
  }

  &-right {
    top: -100%;
    right: 0;
  }

  &-bottom {
    right: -100%;
    bottom: 0;
  }

  &-left,
  &-right {
    width: 2px;
    height: 100%;
  }

  &-top,
  &-bottom {
    height: 2px;
    width: 100%;
  }
}

@keyframes show-strat {
  to {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
