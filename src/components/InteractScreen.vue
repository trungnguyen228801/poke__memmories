<template>
  <h1>Interact comming...</h1>
  <div class="screen">
    <div class="screen__inner" :style="{width: `${(((((920 - 16*4 ) / Math.sqrt(cardContext.length)) - 16) * 3 / 4)+16) * Math.sqrt(cardContext.length)}px`}">
      <app-card-flip v-for="(card, index) in cardContext" :key="index" :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }" @onFlip="checkRule($event)" :ref="`card-${index}`" :cartContext="cardContext"/>
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";

export default {
  props: {
    cardContext: {
      type: Array,
      default: function () {
        return [];
      }
    }
  },
  data() {
    return {
      rules: [],
    };
  },
  components: {
    AppCardFlip: CardFlip,
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      if (this.rules.includes(card)) return false;
      this.rules.push(card);
      console.log(card);
      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        console.log("right");

        this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisableMode();
        this.rules = [];

        const disabledElements = document.querySelectorAll(".screen .card.disabled");
        if (disabledElements && disabledElements.length == this.cardContext.length - 2) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }

      } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
        console.log("wrong");
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);
      } else {
        return false;
      }
    }
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);


}

.screen__inner{
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
