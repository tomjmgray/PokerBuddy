<template>
    <div class="card" :class="[suit, small?'small':'normal']">
        <div class="d-flex flex-column align-items-center">
            <span>{{val}}</span>
            <img :src="`/images/${suit}.svg`" :alt="`${suit}`" :key="index" width="30">
        </div>
        
    </div>
</template>
<script>
export default {
    props: ['suit', 'value', 'small'],
    name: 'Card',
    data() {
        return {
            val: this.value
        }
    },
    beforeMount() {
        if (this.value == '13') {
            this.val = 'K' 
        } else if (this.value == '12') {
            this.val = 'Q' 
        } else if (this.value == '11') {
            this.val = 'J' 
        } else if (this.value == '1') {
            this.val = 'A' 
        } else {
            this.val = this.value
        }
    },
    watch: {
        value(newVal) {
            if (this.value == '13') {
                this.val = 'K' 
            } else if (this.value == '12') {
                this.val = 'Q' 
            } else if (this.value == '11') {
                this.val = 'J' 
            } else if (this.value == '1') {
                this.val = 'A' 
            } else {
                this.val = this.value
            }
        }
    }
}
</script>
<style>
    .d {
        color: blue;
    }    
    .h {
        color: red;
    }
    .c {
        color: green;
    }
    .s {
        color: black
    }
    .card {
        
        border-radius: 5px;
        border: 1px solid black;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
        
        margin: 5px;
    }
    .small {
        width: 35px;
        height: 50px;
        font-size: 16px;
    }
    .normal {
        width: 70px;
        height: 100px;
        font-size: 32px;
    }

</style>