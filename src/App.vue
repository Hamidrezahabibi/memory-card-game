<template> 
  <div class="timer">
    <h3>TimeGame: {{ countDown }}</h3>
  </div>
  <div class="game-board">
    <card v-for="(card, index) in cardList"
          :key="`card-${index}`" 
          :matched="card.matched"
          :value="card.value" 
          :visible="card.visible" 
          :position="card.position" 
          @select-card="flipCard"
    />
    <div>
      <button @click="restartGame" class="botton">Restart</button>
    </div>
    <div>
      <button @click="countDownTimer" class="botton">Start</button>
    </div>
  </div>
  
</template>

<script>
  //import Timer from '@/components/Timer.vue'
  import _ from 'lodash'
  import { computed, ref, watch } from 'vue'
  import Card from '@/components/Card.vue'
  export default {
    name: "App",
    components: {
      Card,
      //Timer
    },
    setup(){
      const cardList = ref([])
      const userSelection = ref ([])
      const countDown = ref(60)
      //const status = ref ('')

      const countDownTimer = () => {
        if(countDown.value >= 1){
          setTimeout(() => {
            countDown.value -= 1
            countDownTimer()
          }, 1000)
        }
      }
      const status = computed (() => {
        if (remainingPairs.value === 0){
          return "Player Wins!"
          //console.log("Player Wins!")
        } else {
          return `Remaining Pairs: ${remainingPairs.value}`
        }
      })

      const remainingPairs = computed(() => {
        const remainingCards = cardList.value.filter(card => card.matched === false).length
        //console.log(remainingCards);
          return remainingCards / 2
      })

      const shuffleCards = () => {
        cardList.value = _.shuffle(cardList.value)
      }

      const restartGame = () => {
        shuffleCards()
        countDown.value = 60

        cardList.value = cardList.value.map ((card, index) => {
          return {
            ...card,
            matched: false,
            position: index,
            visible: false
          }
        })
      }

      const cardItems = ["product-1", "product-2", "product-3", "product-4", "product-5", "product-6", "product-7", "product-8"]

      cardItems.forEach(item => {
        cardList.value.push({
          value: item,
          isible: false,
          position: null,
          matched: false,
        })
        cardList.value.push({
          value: item,
          isible: false,
          position: null,
          matched: false,
        })
      })

      cardList.value = cardList.value.map((card,index) => {
        return{
          ...card,
          position: index
        }
      })

      const flipCard = payload => {
        cardList.value[payload.position].visible = true

        if (userSelection.value[0]){
          if(userSelection.value[0].position === payload.position && userSelection.value[0].faceValue === payload.faceValue){
            return
          } else {
            userSelection.value[1] = payload
          }
        } else {
          userSelection.value[0] = payload
        }
      }

      watch (
        userSelection,
        currentValue => {
          if (currentValue.length === 2){
            const cardOne =currentValue[0]
            const cardTwo =currentValue[1]

            if (cardOne.faceValue === cardTwo.faceValue){
              cardList.value[cardOne.position].matched = true
              cardList.value[cardTwo.position].matched = true
            } else {
              setTimeout(() => {
                cardList.value[cardOne.position].visible = false
                cardList.value[cardTwo.position].visible = false
              }, 2000)
              
            }
            
            userSelection.value.length = 0
          }
        },
        { deep: true }
      )

      return{
        cardList,
        flipCard,
        userSelection,
        status,
        shuffleCards,
        restartGame,
        countDownTimer,
        countDown
      }
    }
  }
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}


.game-board {
  display: grid;
  grid-row: 4;
  grid-column: 3;
  grid-template-columns: 120px 120px 120px 120px;
  grid-template-rows: 120px 120px 120px 120px;
  grid-column-gap: 24px;
  grid-row-gap: 24px; 
  justify-content: center;
  margin-top: 60px;
}
.botton {
  background-color: #b2c6d1;
  color: #0e0f10;
  padding: 0.75rem 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  margin-left: 240px;
  margin-top: 30px;
  font-weight: bold;
}
</style>