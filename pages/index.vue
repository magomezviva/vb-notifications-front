<template>
  <v-layout column justify-center>
    <v-flex xs12 sm12 md12>
      <img src="https://content.vivaaerobus.com/Upload-VB/vb-logo.png" alt="Viva Aerobus">
    </v-flex>
    <v-flex xs12 sm12 md12 v-if="graphData !== null">
      <template v-for="(i, index) of graphData.GetAccounts.docs">
        <Account :data="i" :key="index" />
      </template>
    </v-flex>
    <v-btn @click="getAccountsData" :loading="isLoading" color="green" dark fixed bottom right fab>
      <v-icon>mdi-reload</v-icon>
    </v-btn>
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
      graphData: null,
      limit: 100,
      page: 1,
      isLoading: true,
      buttonLoading: false,
    }
  },
  head() {
    return {
      title: "Status Viva Aerobus",
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        // {
        //   hid: 'description',
        //   name: 'description',
        //   content: 'My custom description'
        // }
      ]
    }
  },
  methods: {
    async getAccountsData() {
      this.isLoading = true;
      try {
        const url = ''
        const query = `
      {
       GetAccounts(page: ${this.page}, limit: ${this.limit}){
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
          data: {
            query
          }
        });
        this.graphData = data.data.data;
        this.graphData.GetAccounts.docs = this.graphData.GetAccounts.docs.sort((a, b) => (a.name > b.name) ? 1 : -1);
        this.isLoading = false;
      } catch (error) {
        this.isLoading = true;
        console.warn(error);
      }
    }
  },
  mounted() {
    this.getAccountsData();
  }
}
</script>
