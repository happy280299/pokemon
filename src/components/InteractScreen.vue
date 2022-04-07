<template>
  <div class="content">
    <div class="screen">
      <h1>Interact</h1>
      <CardFlip v-for="(card, index) in cardConText"
      :key="index"
      :ref="`card-${index}`"
      :imgBackFaceUrl="`images/${card}.png`"
      :card="{index, value: card}" @onFlip="checkRule($event)"/>
    </div>
  </div>
</template>


<script>
import CardFlip from "./CardItem.vue"
export default {
  props: {
    cardConText: {
      type: Array,
      default: function() {
        return [];
      }
    }
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
    }
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);

      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        console.log("Right...")
        //add class
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisabledMode()
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisabledMode()
        this.rules = [];
        const  disableElement = document.querySelectorAll(".screen .card.disabled")
        if (disableElement && disableElement.length === this.cardConText.length - 2) {
          setTimeout(() => {
            this.$emit("onFinish")
          },920);
        }
      }else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
        setTimeout(() => {
          //close card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard()
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard()
          this.rules = []
        }, 800);
      }else return false;
    },
  }
}
</script>

<style scoped>
  .content {
    background-color: #000;
    width: 100%;
    height: 100vh;
  }
  .screen {
    width: 800px;
    margin: 0 auto;
    height: 100%;
  }
  .card {
    box-shadow: 0 3px 10px rgba(255,255,255,.5);
  }

</style>