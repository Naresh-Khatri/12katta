<template>
  <q-page class="flex flex-center">
<GameBoard />
  </q-page>
</template>

<script>
import GameBoard from '../components/GameBoard.vue'
import io from 'socket.io-client'

export default {
  name: 'PageIndex',
  components: { GameBoard},

  data(){
    return{
    socket:{},
  }},
  created(){
    // this.socket = io('ws://localhost:3000')
  },
  mounted(){
    // console.log(this.socket)
    // this.socket.on(`connect`,()=>{console.log(`connected!`)})
  }
}
</script>
