<template>
    <section >

<!-- Title bar -->        
    <section class="hero is-info is-medium is-bold">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">Advanced Matching Betting</h1>
          <h2 class="subtitle">The best Matched Betting Calculator to make <strong>guaranteed</strong> profit every time</h2>
        </div>
      </div>   


<!-- Navigation Tabs --> 
    <div class = "hero-foot">
        <nav class="tabs is-centered is-large is-boxed">
            <ul>
                <li  :class="{'is-active': activeTab === 1}" @click="activeTab = 1">
                    <a @click="page = 'whatIs'" >Guide</a>
                </li>
                <li :class="{'is-active': activeTab === 2}" @click="activeTab = 2">
                    <a @click="page = 'matchedBetting'">Calculator</a>
                </li>
                <li :class="{'is-active': activeTab === 3}" @click="activeTab = 3">
                    <a @click="page = 'bonusBets'">Bonus Bets</a>
                </li>
            </ul>
        </nav>
    </div>
    </section>   
 <!-- Change page based on what is set from navigation tab-->   
        <section>
            <matched-betting 
                v-if="page === 'matchedBetting'"
                @output="sendBettingData"                                    
                >
            </matched-betting>
            <betting-results
                v-else-if="page === 'bettingResults'"
                :betting_data="betting_data"
                >
            </betting-results>
            <what-is
                v-else-if="page === 'whatIs'"
                >
            </what-is>
            <bonus-bets
                v-else-if="page === 'bonusBets' "
                >
            </bonus-bets>
        </section>

<!-- Footer -->
         <footer class="footer">
            <div class="content has-text-centered">
                <p>
                    <strong>Advanced Matched Betting</strong> by jCStudio. For all inquiries mail <a href="advancedmatchedbetting@gmail.com">advancedmatchedbetting@gmail.com</a>
                </p>
            </div>
        </footer>
 
    </section>
</template>

<script>
import matchedBetting from './pages/matchedBetting.vue'
import bettingResults from './pages/bettingResults.vue'
import whatIs from './pages/whatIs.vue'
import bonusBets from './pages/bonusBets.vue'
export default {
    components: {
        'matched-betting': matchedBetting,
        'betting-results': bettingResults,
        'what-is': whatIs, 
        'bonus-bets': bonusBets 
    },
    data () {
        return {
            page: 'matchedBetting',
            betting_data: null,
            activeTab: 2
        }
    },
    methods: {
        sendBettingData (data) {
            this.betting_data = data
            console.log(data)
            this.page = 'bettingResults'
        },
        addPage(page_name) {
            this.page = page_name
        }
        
    }
}
</script>

<style lang="css">

.navbar {
    overflow: hidden;
    background-color: #333;
    position: fixed; /* Set the navbar to fixed position */
    top: 0; /* Position the navbar at the top of the page */
    width: 100%; /* Full width */
}
</style>