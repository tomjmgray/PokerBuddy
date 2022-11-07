<script>
import deckExp from '../assets/deck';

export default {
    createDeck() {
        return [
            {value: 1, suit: 'h'},
            {value: 2, suit: 'h'},
            {value: 3, suit: 'h'},
            {value: 4, suit: 'h'},
            {value: 5, suit: 'h'},
            {value: 6, suit: 'h'},
            {value: 7, suit: 'h'},
            {value: 8, suit: 'h'},
            {value: 9, suit: 'h'},
            {value: 10, suit: 'h'},
            {value: 11, suit: 'h'},
            {value: 12, suit: 'h'},
            {value: 13, suit: 'h'},
            {value: 1, suit: 's'},
            {value: 2, suit: 's'},
            {value: 3, suit: 's'},
            {value: 4, suit: 's'},
            {value: 5, suit: 's'},
            {value: 6, suit: 's'},
            {value: 7, suit: 's'},
            {value: 8, suit: 's'},
            {value: 9, suit: 's'},
            {value: 10, suit: 's'},
            {value: 11, suit: 's'},
            {value: 12, suit: 's'},
            {value: 13, suit: 's'},
            {value: 1, suit: 'd'},
            {value: 2, suit: 'd'},
            {value: 3, suit: 'd'},
            {value: 4, suit: 'd'},
            {value: 5, suit: 'd'},
            {value: 6, suit: 'd'},
            {value: 7, suit: 'd'},
            {value: 8, suit: 'd'},
            {value: 9, suit: 'd'},
            {value: 10, suit: 'd'},
            {value: 11, suit: 'd'},
            {value: 12, suit: 'd'},
            {value: 13, suit: 'd'},
            {value: 1, suit: 'c'},
            {value: 2, suit: 'c'},
            {value: 3, suit: 'c'},
            {value: 4, suit: 'c'},
            {value: 5, suit: 'c'},
            {value: 6, suit: 'c'},
            {value: 7, suit: 'c'},
            {value: 8, suit: 'c'},
            {value: 9, suit: 'c'},
            {value: 10, suit: 'c'},
            {value: 11, suit: 'c'},
            {value: 12, suit: 'c'},
            {value: 13, suit: 'c'},
        ]
    }, 
    shuffle(deck) {
        // console.log('SHUFFLES')
        // let deck = deckExp
        // console.log('Deck', deck)
        let newDeck = []
        for (let i = 0; i < 52; i++) {
            let index = Math.floor(Math.random() * deck.length); 
            // console.log(index)
            const card = deck.splice(index, 1);

            newDeck.push(card[0])
        };
        return newDeck
    },
    checkStraightFlush(pool) {
        let straightFlush = false;
        pool.forEac
    },
    checkPairs(givenPool) {
        const pool = [];
        givenPool.forEach(card => {
            pool.push(card)
        })
        // console.log('pool for checkPairs', pool);
        let hand = [];
        let pair = false;
        let twoPair = false;
        let trips = false;
        let fullHouse = false;
        let quads = false;
        let handName = ''
        for (let i = 0; i < pool.length; i++) {
            // console.log(pool[i])
            //nothing
            if ( i == pool.length - 1 || pool[i].value !== pool[i+1].value) {
                continue
            //pair
            } else if (pool[i].value == pool[i+1].value) {
                //makes sure not double checking trips
                if (i > 0 && pool[i].value !== pool[i-1].value ) {
                    //adds full house
                    if (trips == true) {
                        fullHouse = true
                    } 
                    //adds two pair
                    if (pair == true) {
                        twoPair = true;
                    }
                    
                };
                //makes pair true
                pair = true;
                //err handle for trips overflow
                if (i == pool.length - 2) {
                    continue 
                //checks trips
                } else if (pool[i].value == pool[i+2].value) {
                    //adds full house
                    if ( i > 0 && pool[i].value !== pool[i-1].value && pair == true) {
                        fullHouse = true;
                    };
                    //err handle for quads overflow
                    if (i == pool.length - 3) {
                        continue 
                    //adds quads
                    } else if (pool[i].value == pool[i+3].value) {
                        quads = true
                    }
                    // adds trips
                    trips = true;
                }
            };
        };
        // console.log(pair, twoPair, trips, fullHouse, quads,)
        if (quads !== false) {
            for (let i = 0; i < pool.length; i++) {
                if (pool[i].value == pool[i + 3].value) {
                    const obj = pool.splice(i, 4);
                    hand = hand.concat(obj)
                    hand.push(pool[pool.length - 1]);
                    break
                }
            }
            
                handName= 'Quads'
         
        } else if (fullHouse !== false) {
            for (let i = pool.length - 1; i > -1; i--) {
                if (i > 1 && pool[i].value == pool[i - 2].value) {
                    const obj = pool.splice(i - 2, 3);
                    hand = hand.concat(obj)
                    i -= 2;
                    if (hand?.length == 5) {
                        break
                    }
                } else if ( i > 0 && pool[i].value == pool[i -1].value && hand.length < 2) {
                    const obj = pool.splice(i - 1, 2);
                    hand = hand.concat(obj)
                    i--
                    if (hand?.length == 5) {
                        break
                    }
                }
            };
           
                handName = 'Full House'
          
        } else if (trips !== false) {
            for (let i = pool.length - 1; i > -1; i--) {
                if (i > 1 && pool[i].value == pool[i-2].value) {
                    const obj = pool.splice(i - 2, 3);
                    hand = hand.concat(obj)
                    hand.push(pool[pool.length - 1]);
                    hand.push(pool[pool.length - 2]);
                    if (hand?.length == 5) {
                        break
                    }
                }
            }
            
                handName = 'Trips'
        
        } else if (twoPair !== false) {
            for (let i = pool.length - 1; i > -1; i--) {
                if (i > 0 && pool[i].value == pool[i-1].value) {
                    const obj = pool.splice(i - 1, 2);
                    hand = hand.concat(obj)
                    i--
                    if (hand?.length == 4) {
                        hand.push(pool[pool.length - 1]);
                        if (hand?.length == 5) {
                            break
                        }
                    }
                    
                   
                    
                }
            }
            
            handName = 'Two Pair'
           
        } else if (pair !== false) {
            for (let i = pool.length - 1; i > -1; i--) {
                if (i > 0 && pool[i].value == pool[i-1].value) {
                    const obj = pool.splice(i - 1, 2);
                    console.log('OBJ', obj)
                    hand = hand.concat(obj)
                    hand.push(pool[pool.length - 1]);
                    hand.push(pool[pool.length - 2]);
                    hand.push(pool[pool.length - 3]);
                    if (hand?.length == 5) {
                        break
                    }
                }
                
                    

                // console.log('HAND', hand)
                // return {
                //     hand: hand,
                // }
            }
            handName = 'Pair';
        } else {
            const obj = pool.splice(pool.length - 6, 5)
            hand = hand.concat(obj)
            // console.log('HAND', hand)
            // return 
            //     hand: hand,
            handName = 'High Card'
            // 
        }
        console.log({hand: hand, handName: handName})
        return {hand: hand, handName: handName}
    },
    calculateHand(hand, community) {
        
        let pairAnswer = 'High Card';
        let straight = false;
        let flush = false;
        
        let straightFlush = false;
        let pool = (community?.concat(hand)).sort((a, b) => a.value - b.value);
        const finishedEval = this.checkPairs(pool)

        
        return {
            hand: finishedEval.hand,
            handName: finishedEval.handName,
            
        }
    }

}
</script>   