<template>
<h1>Ticket WHore</h1>
  <TicketBox></TicketBox>

</template>

<script>
import TicketBox from './components/TicketBox.vue'

export default {
  name: 'App',
  components: { TicketBox }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
