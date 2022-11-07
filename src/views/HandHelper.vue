<template>
<main>
    <h1>Hand Helper</h1>
    <!-- <button @click="shuffle" class="btn btn-secondary">Shuffle</button> -->
    
    <input type="number" v-model="numberOfPlayers">
    <button  @click="deal"  class="btn btn-primary mx-3">Deal</button>
    <button  @click="flop"  class="btn btn-primary mx-3" v-if="communityTurn == 'Flop'">Flop</button>
    <button  @click="turn"  class="btn btn-primary mx-3" v-if="communityTurn == 'Turn'">Turn</button>
    <button  @click="river"  class="btn btn-primary mx-3" v-if="communityTurn == 'River'">River</button>
    <button  @click="show"  class="btn btn-primary mx-3" v-if="communityTurn == 'Show'">Show</button>
    <!-- <div class="d-flex flex-wrap">
        <Card v-for="card, i in deck" :key="i" :suit="card.suit" :value="card.value" />
    </div> -->
    <div v-if="players?.length > 0 && players[0].hand?.length > 0" class="d-flex flex-row flex-wrap">
        <div v-for="(player, index) in players" :key="index" class="d-flex flex-column m-3">
          
            <h4>Player {{index +1 }}</h4>
            <p v-if="player?.handName">{{player?.handName}}</p>
            <div class="d-flex" v-if="player?.finishedHand?.length < 1">
                
                <Card v-for="card, index in player.hand" :key="index" :suit="card.suit" :value="card.value" :small="false" />
            </div>
            <div class="d-flex" v-else>
                <Card v-for="card, index in player.finishedHand" :key="index" :suit="card.suit" :value="card.value" :small="true" />
            </div>
            
        </div>
            
       
    </div>
    <div v-if="community?.length > 0 " class="d-flex flex-column">
        <h4>Board</h4>
        <div  class="d-flex flex-row flex-wrap">
            <div v-for="(card, index) in community" :key="index" class="d-flex flex-column my-3">
                <Card  :suit="card.suit" :value="card.value" />
            </div>
        </div>
        
            
       
    </div>
    

</main>
</template>



<script>
import deckExp from '../assets/deck';
import lodash from 'lodash'
import Card from '../components/Card.vue'
import Methods from '../components/Methods.vue'
export default {
    Name: 'HandHelper',
    components: {Methods, Card},
    data() {
        return {
            deck: null,
            numberOfPlayers: 2,
            players: [
                
            ],
            community: [],
            communityTurn: 'Flop'
        }
    },
    methods: {
        start() {
            // console.log('DeckEx', deckExp)
            this.players = [];
            this.community = [];
            this.communityTurn = 'Flop'
            for (let i = 0; i < this.numberOfPlayers; i++) {
                this.players.push({hand: []})
            }
            const deck = Methods.shuffle(Methods.createDeck());
            this.deck = deck;
            console.log(this.deck)
        },
        shuffle() {
            const newDeck = Methods.shuffle(this.deck)
            this.deck = newDeck
        },
        dealOne() {
            const card = this.deck.splice(Math.floor(Math.random() * this.deck.length), 1)
            console.log(card);
            return card[0]
        },
        deal() {
            console.log(this.start)
            this.start()
            for (let i = 0; i < this.numberOfPlayers; i++) {
                

                
                let card1 = this.dealOne()
                let card2 = this.dealOne()
                
                this.players[i].hand.push(card1);
                this.players[i].hand.push(card2);
                console.log(this.players[i])
            };
            
        }, 
        watch: {
            numberOfPlayers(newVal, oldVal) {
                console.log(newVal);
                console.log(oldVal)
                if (newVal > oldVal) {
                    for (let i = 0; i < newVal - oldVal; i++) {
                        this.players.push({hand: []})
                    }
                } else if (newVal < oldVal) {
                    for (let i = 0; i < newVal - oldVal; i++) {
                        this.players.pop()
                    }
                }
            }
        },
        flop() {
            this.community.push(this.dealOne());
            this.community.push(this.dealOne());
            this.community.push(this.dealOne());
            this.communityTurn= 'Turn'

        },
        turn() {
            this.community.push(this.dealOne());
            this.communityTurn= 'River'
        },
        river() {
            this.community.push(this.dealOne());
            this.communityTurn= 'Show'
        },
        show() {
            // console.log(this.players)
            this.players?.forEach(player => {
                let answer = Methods.calculateHand(player.hand, this.community);
                console.log(answer)
                player.finishedHand = answer.hand;
                player.handName = answer.handName
                // console.log(answer)
            })
            
        }
    },  
    mounted() {
        this.start()
    },

}

</script>
<style scoped>

</style>