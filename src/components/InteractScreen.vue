<template>
    <div class="screen">
      <count-Times 
      />

      <div class="screen__inner" :style="{
        width:  `${((((screenHeight - 10 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 )/ 4 + 16) * Math.sqrt(cardsContext.length)}px`
        
      }">
        <card-memmory 
        v-for="(card, index) in cardsContext" 
        :key="'card-' + index"
        :ref="'card-' + index"   
        :imgBackFaceUrl="`/images/${card}.png`"
        :card="{index , value : card}"
        :cardsContext = "cardsContext"
        :isClickable="isClickable" 
        @changeFlip="checkRule($event)"
        />
      </div>
    </div>
</template>
<script>
import Card from "./Card.vue";
import countTimes from "./countTimes.vue";

  export default{
    props:{
      cardsContext : {
        type : Array ,
        default : function(){
          return [];
        }
      },
    },
    components:{
      CardMemmory: Card,
      countTimes : countTimes,
    },
    data(){
      return {
        rules : [],
        isClickable: true, // Đảm bảo rằng bạn đã khai báo biến isClickable ở đây.
        screenHeight: window.innerHeight ,
        screenWitdh: window.innerWidth,
      }
    },
    methods : {
      checkRule(card){
        if (this.rules.length === 2) return false;
        this.rules.push(card);
        if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value){
          if(this.rules[0].index == this.rules[1].index){
            this.rules = [];
            return false ;
          }
          // add class 'disabled' to component card
          this.$refs[`card-${this.rules[0].index}`][0].onEnableDisabledCard(); // Sửa lại cách truy cập $refs
          this.$refs[`card-${this.rules[1].index}`][0].onEnableDisabledCard(); // Sửa lại cách truy cập $refs
          // reset rules to []
          this.rules = [];
          const disabledElements = document.querySelectorAll(".screen .card.disabled");
          if(disabledElements.length === this.cardsContext.length - 2){
            setTimeout(() => {
              this.$emit("onFinish");
            }, 920)
          }
         
        }else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value){
          this.isClickable = false;
          setTimeout(() => {
            this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard(); // Sửa lại cách truy cập $refs
            this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard(); // Sửa lại cách truy cập $refs
            this.rules = [];
            this.isClickable = true;
          }, 800)

        }else{
          return false ;
        }
      }
    }
  }
</script>
<style lang="css" scoped>
  .screen{
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