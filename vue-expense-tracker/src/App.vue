/* eslint-disable */
<template>
  <Header />
  <div class="container">
  <Balance />
  <IncomeExpenses />
  </div>
</template>
<script lang = 'ts'>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
export default {
  components:{
    Header,
    Balance,
    IncomeExpenses,
  },
  name: 'App',
  data() {
    return {
      
      
    }
  }
}

</script>

<style>

</style>
