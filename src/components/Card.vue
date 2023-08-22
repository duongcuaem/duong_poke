<template>
    <div class="card" :class="{disabled : isDisabled}" :style="{
        height:`${(screenHeight - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px` ,
        width:`${(((screenHeight - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 )/ 4}px`,
        perspective: `${(((screenHeight - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 )/ 4 * 2}px`
    }">
        <div 
        class="card__inner"  
        @click="onToggleFlipCard" 
        :class="{'is-flipped' : isFlipped}">
            <div class="card__face card__face--front"> 
                <div class="card__content"
                :style="{
                       'background-size': `${
                        (((screenHeight - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
                        4 /
                        3
                        }px ${
                        (((screenHeight - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
                        4 /
                        3
                        }px`,
                    }">
                </div>
            </div>
            <div class="card__face card__face--back"> 
                <div
                    class="card__content"
                    :style="{backgroundImage: `url(/src/assets/${(imgBackFaceUrl)})`,}"                    
                ></div>
            </div>
        </div>
    </div>
</template>

<script>
    export default{
        props: {
            card:{
                type: [Array, String, Number, Object],
            },
            imgBackFaceUrl : {
                type : String ,
                required : true,
            },
            cardsContext:{
                type : Array,
                default : function(){
                    return []
                }
            },
            isClickable: { // Đặt prop isClickable để nhận giá trị từ component cha
                type: Boolean,
                default: true, // Đặt giá trị mặc định là true hoặc false tùy theo nhu cầu
            },
        },
        data(){
            return{
                isDisabled : false,
                isFlipped : false,
                screenHeight: window.innerHeight - 30 ,
            };
        }, 
        methods: {
            onToggleFlipCard(){
                if(this.isDisabled || !this.isClickable){
                    return false
                } 
                this.isFlipped = !this.isFlipped
                this.$emit("changeFlip", this.card);
            },
            onFlipBackCard(){
                this.isFlipped = false ;
            },
            onEnableDisabledCard(){
                this.isDisabled = true ;
            },

        },
    }
   
</script>

<style lang="css" scoped>
    .card {
        display: inline-block;
        margin-right: 1rem;
        margin-bottom:  1rem;
        height: 120px;
       }

    .card.disabled .card__inner{
        cursor: default;
    }

    .card__inner {
        height: 100%;
        width: 100%;
        transition: transform 1s;
        transform-style: preserve-3d;
        cursor: pointer;
        position: relative;
    }

    .card__inner.is-flipped {
        transform: rotateY(-180deg);
    }

    .card__face {
        position: absolute;
        width: 100%;
        height: 100%;
        backface-visibility: hidden;
        overflow: hidden;
        border-radius: 1rem;
        padding: 1rem;
        box-shadow: 0px 3px 10px 3px rgba(0, 0, 0, 0.2);
    }

    .card__face--front .card__content {
        background: url("../assets/images/icon_back.png") no-repeat center center;
        height: 100%;
        width: 100%;
    }

    .card__face--back {
        background-color: var(--light);
        transform: rotateY(-180deg);
    }

    .card__face--back .card__content{
      height: 100%;
      width: 100%;
      background-size: contain;
      background-repeat: no-repeat;
      background-position: center center;
    }

</style>