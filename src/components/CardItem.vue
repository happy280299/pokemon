<template>
  <div class="card" :class="{ disabled: isDisabled }">
    <div class="card-inner" :class="{'is-flipped': isFlipped}" @click="onToggle()">
      <div class="card-front card-face">
        <div class="card-content"></div>
      </div>
      <div class="card-face card-back">
        <div class="card-content" :style="{ backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`}"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: [String, Number, Array, Object],
    },
    imgBackFaceUrl: {
      type: String,
      required: true,
    }
  },
  data() {
    return {
      isDisabled: false,
      isFlipped: false,
    }
  },
  methods: {
    onToggle() {
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnableDisabledMode() {
      this.isDisabled = true;
    }
  },
};
</script>

<style lang="css" scoped>
  .card {
    display: inline-block;
    margin: 1rem;
    width: 120px;
    height: 150px
  }
  .card-inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
  }

  .card.disabled .card-content {
    cursor: default;
  }

  .card-inner.is-flipped {
    transform: rotateY(-180deg)
  }
  .card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 6px 3px rgba(0,0,0,.2);
  }

  .card-front .card-content {
    background: url("../assets/images/icon_back.png") no-repeat center;
    background-size: 40px 40px;
    height: 100%;
    width: 100%;
  }
  .card-back {
    background: var(--light);
    transform: rotateY(-180deg)
  }

  .card-back .card-content {
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
  }
</style>