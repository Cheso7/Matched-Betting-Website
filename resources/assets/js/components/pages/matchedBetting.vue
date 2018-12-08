<template>
  <div>
<!-- Bookmaker Site Section -->
    <body>
      <br>
      <section class="container">
        <div class="box">

          <h1 class="title has-text-left">Bookmaker</h1>

          <div class="columns">
            <div class="column">
              <div class="field">
                <label class="label has-text-left">Deposit Amount</label>
                <div class="control">
                  <input class="input" type="number" placeholder="$0.00" v-model="betting_data.deposit_amount">
                </div>
                <p class="help has-text-left">Enter the amount of money deposited.</p>
              </div>
            </div>

            <div class="column">
              <div class="field">
                <label class="label has-text-left">Bonus Bet Amount</label>
                <div class="control">
                  <input class="input" type="number" placeholder="$0.00" v-model="betting_data.bonus_amount">
                </div>
                <p class="help has-text-left">Enter the value of the bonus bet received.</p>
              </div>
            </div>

            <div class="column">
              <div class="field">
                <label class="label has-text-left">Back Bet Odds</label>
                <div class="control">
                  <input class="input" type="number" placeholder="$0.00" v-model="betting_data.back_odds">
                </div>
                <p class="help has-text-left">Enter the decimal value of the back bet odds (e.g. $3.30)</p>
              </div>
            </div>            
          </div>


          <div class="columns">
            <div class="column">
              <div class="field">
                <label class="label has-text-left">Deposit Turnover Requirement</label>
                <div class="control">
                  <input class="input" type="number" placeholder="1x" v-model="betting_data.deposit_turnover">
                </div>
                <p class="help has-text-left">Specify the turnover requirement for the deposit (e.g. 1x)</p>
              </div>
            </div>

            <div class="column">
              <div class="field">
                <label class="label has-text-left">Bonus Bet Turnover Requirement</label>
                <div class="control">
                  <input class="input" type="number" placeholder="1x" v-model="betting_data.bonus_bet_turnover">
                </div>
                <p class="help has-text-left">Specify the turnover requirement for the bonus bet (e.g. 1x)</p>
              </div>
            </div>

            <div class="column">
              <div class="field">
                <label class="label has-text-left">Bonus Bet Winnings Turnover Requirement</label>
                <div class="control">
                <input class="input" type="number" placeholder="1x" v-model="betting_data.bonus_bet_winnings_turnover">
                </div>
                <p class="help has-text-left">Specify the turnover requirement for the bonus bet winnings (e.g. 1x)</p>
              </div>
            </div>
          </div>

          <div class="columns">


            <div class="column">
              <label class="label has-text-left">Does the bookmaker return the bonus bet stake?</label>

              <div class="control">
                <label class="radio">
                  <input type="radio" :value="true" v-model="betting_data.stake_return">
                  Yes
                </label>
                <label class="radio">
                  <input type="radio" :value="false" v-model="betting_data.stake_return">
                  No
                </label>
              </div>
            </div>
            
            </div>

            <div class="columns">

              <div class="column"> </div>
             </div>
        </div>
      </section>
    </body>

    <br>

<!-- Lay Bet Site Section -->
    <body v-if="page != 'bettingResults'">
      <section class="container">
        <div class="box">

      <h1 class="title has-text-left">Betting Exchange</h1>

      <div class="columns">
        <div class="column">
          <div class="field">
            <label class="label has-text-left">Lay Bet Odds</label>
            <div class="control">
              <input class="input" type="number" placeholder="$0.00" v-model="betting_data.lay_odds">
            </div>
            <p class="help has-text-left">Enter the decimal value of the lay bet odds (e.g. $3.65)</p>
          </div>
        </div>
        <div class="column">
          <div class="field">
            <label class="label has-text-left">Lay Bet Commission</label>
            <div class="control">
              <input class="input" type="number" placeholder="5%" v-model="betting_data.lay_bet_commission">
            </div>
            <p class="help has-text-left">Enter the commission charged by the Bet Exchange (e.g. 5%)</p>
          </div>
        </div>
        <div class="column">
        </div>
        </div>
        </div>

        <br>

      </section>

      <section>
        <body>
              <a class="button" @click="showResults()" >Calculate my bet</a>
        </body>
      </section>
      
        <br />

    </body>
</div>

</template>

<script>
import bettingResults from "./bettingResults.vue";
export default {
  components: {
    bettingResults,
  },
  data() {
    return {
     page: 'matchedBetting',
      betting_data: {
        deposit_turnover: null,
        deposit_amount: null,
        bonus_amount: null,
        bonus_bet_turnover: null,
        bonus_bet_winnings_turnover: null,
        lay_odds: null,
        lay_bet_commission: null,
        back_odds: null,
        stake_return: false,
      }
    }
  },
  methods: {
    showResults () {
      this.$emit('output', this.betting_data)
    }, 

    // checkFields(){
    //   var arrayLength = betting_data.length;
    //   var fieldsfull = null
    //   for (var i = 0; i < arrayLength; i++) {
    //   if (betting_data[i] === null){
    //   fieldsfull = true
    //   }
    //   }
    //   return fieldsfull
    //   console.log(this.betting_data)
    //   console.log(this.fieldsfull)
    // }

  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
