<template>
  <div class="container">
    <div>   
      <br>   
      <section class="box" v-if="calc1 != null">
        <p class="title is-3">Your Matched Bet</p>
        <table class="table is-fullwidth">
          <thead>
            <th></th>
            <th class="has-text-centered">Lay Bet</th>
            <th class="has-text-centered">Back Bet</th>
          </thead>
          <tbody>
            <tr>
              <td class="is-narrow"><strong>Bet Amount</strong></td>
              <td class="has-text-centered">${{(calc1.lay_bet).toFixed(2)}} with a Liability of ${{(calc1.liability).toFixed(2)}}</td>
              <td class="has-text-centered">${{(calc1.back_bet).toFixed(2)}} of which your Bonus Bet is ${{(parseInt(betting_data.bonus_amount)).toFixed(2)}}</td>
            </tr>
            <tr>
              <td><strong>Return</strong></td>
              <td class="has-text-centered">${{(calc1.return_lay).toFixed(2)}} if the Lay Bet wins</td>
              <td class="has-text-centered">${{(calc1.return_back).toFixed(2)}} if the Back Bet wins</td>
            </tr>
            <tr :class="{'has-background-success':calc1.profit_back > 0, 'has-background-danger':calc1.profit_back <= 0 || calc1.profit_lay <= 0}" class="has-text-light">
              <td ><strong class="has-text-light">Profit</strong></td>
              <td class="has-text-centered">${{(calc1.profit_lay).toFixed(2)}} if the Lay Bet wins</td>
              <td class="has-text-centered">${{(calc1.profit_back).toFixed(2)}} if the Back Bet wins</td>
            </tr>   
          </tbody>
        </table>

        <h2 v-if="calc1.profit_back <= 0 || calc1.profit_lay <= 0"> 
          <strong>Warning:</strong> You are not guaranteed a profit with these odds.
        </h2>

        <body v-if="calc1.profit_back >= 0 && calc1.profit_lay >= 0">
        <h2 v-if="calc1.turnover_bet !=0"> 
          <strong>Turnover Requirements:</strong> You will have a turnover requirement of ${{calc1.turnover_bet}} if the Back Bet wins.
        </h2>

        <h2 v-if="calc1.turnover_bet ==0"> 
          <strong>Turnover Requirements:</strong> You have no turnover requirement remaining, you can withdraw all your funds.
        </h2>
        </body>

      </section>

      <br>

      <section v-if="calc1.turnover_bet !=0" class="box">
        <p class="title is-3">Your Turnover Bet</p>
        <h2 class="subtitle">
          If the Back Bet wins, enter the odds for a second Matched Bet to meet your turnover requirements. 
        </h2>
        <div class="columns">
          <div class="column">
            <div class="field">
              <label class="label">Lay Bet Odds</label>
              <div class="control">
                <input class="input" type="number" placeholder="$0.00" v-model="lay_odds2">
              </div>
            </div>
          </div>
          <div class="column">
            <div class="field">
              <label class="label">Back Bet Odds</label>
              <div class="control">
                <input class="input" type="number" placeholder="$0.00" v-model="back_odds2">
              </div>
            </div>
          </div>
        </div>     
        
        <a class="button is-dark" @click="calculate_bet2()">Tell me what to bet to get my money out!</a>

        <body v-if="calc2.lay_bet != 0">
        <table  class="table is-fullwidth">
          <thead>
            <th></th>
            <th class="has-text-centered">Lay Bet</th>
            <th class="has-text-centered">Back Bet</th>
          </thead>
          <tbody>
            <tr>
              <td class="is-narrow"><strong>Bet Amount</strong></td>
              <td class="has-text-centered">${{(calc2.lay_bet).toFixed(2)}} with a Liability of ${{(calc2.liability).toFixed(2)}}</td>
              <td class="has-text-centered">${{(calc2.back_bet).toFixed(2)}}</td>
            </tr>
            <tr>
              <td><strong>Return</strong></td>
              <td class="has-text-centered">${{(calc2.return_lay).toFixed(2)}} if the Lay Bet wins</td>
              <td class="has-text-centered">${{(calc2.return_back).toFixed(2)}} if the Back Bet wins</td>
            </tr>
            <tr :class="{'has-background-success':calc2.profit_back > 0, 'has-background-danger':calc2.profit_back <= 0 || calc2.profit_lay <= 0}" class="has-text-light">
              <td><strong class="has-text-light">Final Profit</strong></td>
              <td class="has-text-centered">${{(calc2.profit_lay).toFixed(2)}} if the Lay Bet wins</td>
              <td class="has-text-centered">${{(calc2.profit_back).toFixed(2)}} if the Back Bet wins</td>
            </tr>   
          </tbody>
        </table>

        <h2 v-if="calc2.profit_back <= 0 || calc2.profit_lay <= 0"> 
          <strong>Warning:</strong> You are not guaranteed a profit with these odds.
        </h2>
      </body>
      </section> 
      <br>  
    </div>
    
  </div>

</template>

<script>
export default {
  name: 'bettingResults',
  props: ['betting_data'],
  data() {
    return {

      calc1: null,
      calc2: {
        back_bet: 0,
        lay_bet: 0,
        profit_lay: 0,
        profit_back: 0, 
        return_back: 0, 
        return_lay: 0,  
        liability: 0 
      },

      lay_odds2: null, 
      back_odds2: null

    }
  },
  created() {
    if (this.betting_data != null) {
      // Calcuate the data. 
      console.log(this.betting_data)
      this.calculate_bet1()
    }
  },
  methods: {
    calculate_bet1(){
      var data = this.betting_data

      var results = {
        back_bet: 0,
        lay_bet: 0,
        profit_lay: 0,
        profit_back: 0,   
        return_back: 0, 
        return_lay: 0,  
        deposit_bet: 0,
        liability: 0,
        winnings_bet: 0, 
        turnover_bet: 0, 
        bonus_bet: 0
      }

      results.bonus_bet = parseInt(data.bonus_amount)
      results.deposit_bet = data.deposit_turnover*data.deposit_amount
      results.back_bet = results.deposit_bet + results.bonus_bet  

      if (data.stake_return == true) {
        results.return_back = results.back_bet*data.back_odds
      } else {
        results.return_back = results.back_bet*data.back_odds - (data.bonus_amount)
      }
      
      results.lay_bet = (results.return_back)/(data.lay_odds+1-(data.lay_bet_commission/100))
      results.return_lay = results.lay_bet*(1-data.lay_bet_commission/100) + results.lay_bet*data.lay_odds - results.lay_bet
      results.profit_lay = results.return_lay - results.lay_bet*(data.lay_odds - 1) - results.deposit_bet
      results.profit_back = results.return_back - (results.lay_bet*data.lay_odds - results.lay_bet) - results.deposit_bet 
      results.liability = results.lay_bet*data.lay_odds - results.lay_bet

      results.winnings_bet = (results.bonus_bet*data.back_odds -results.bonus_bet)*(data.bonus_bet_winnings_turnover)
      results.turnover_bet = (results.bonus_bet*data.bonus_bet_turnover - results.bonus_bet) + results.winnings_bet

      console.log(results)
      this.calc1 = results

    },

    calculate_bet2(data){
      var data = this.betting_data
      var bet1 = this.calc1

      var results = {
        back_bet: 0,
        lay_bet: 0,
        
        profit_lay: 0,
        profit_back: 0, 

        return_back: 0, 
        return_lay: 0,  

        liability: 0 

      }
        results.back_bet = bet1.turnover_bet
        results.return_back = bet1.turnover_bet*this.back_odds2
        
        results.lay_bet = (results.return_back)/(this.lay_odds2+1-(data.lay_bet_commission/100))
        results.liability = results.lay_bet*this.lay_odds2 - results.lay_bet
        results.return_lay = results.lay_bet*(1-data.lay_bet_commission/100) + results.lay_bet*this.lay_odds2 - results.lay_bet

        results.profit_lay = (results.return_lay + bet1.return_back) - (results.back_bet + bet1.liability + bet1.deposit_bet + results.liability)
        results.profit_back = (results.return_back + bet1.return_back) - (results.back_bet + bet1.liability + bet1.deposit_bet + results.liability)


      console.log(results)
      this.calc2 = results

  }
  }
}
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
