<template>
  <v-layout column justify-center>
    <v-flex xs12 sm12 md12>
      <img src="https://content.vivaaerobus.com/Upload-VB/vb-logo.png" alt="Viva Aerobus">
    </v-flex>
    <v-flex xs12 sm12 md12 v-if="graphData !== null">
      <template v-for="(i, index) of graphData.GetAccounts.docs">
        <Account :data="i" :key="index"/>
      </template>
    </v-flex>
  </v-layout>
</template>

<script>
import Account from '../components/Account'
export default {
  components: {
    Account
  },
  data() {
    return {
      graphData: null
    }
  },
  async mounted() {
    try {
      console.log(this.$axios.defaults.baseURL);
      const url = ''
      const query = `
      {
       GetAccounts(page: 1, limit: 20){
           totalDocs,
           docs{
             key,
             name,
             description,
             notificationResume {
               message
               data
               isError
               createdAt
             }
           },
         }
      }`

      const data = await this.$axios({
        url,
        method: 'post',
        data:{
          query
        }
      });
      this.graphData = data.data.data;
    } catch (error) {
      console.warn(error);
    }
  }
}
</script>
