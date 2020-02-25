<template>
  <div class="card-container">
    <v-card>
      <v-card-title>
        {{ data.name }}
      </v-card-title>
      <v-card-text>
        <template v-if="data.notificationResume.length > 0">
          <v-alert border="bottom" colored-border type="error" v-if="data.notificationResume[0].isError">
            {{ data.notificationResume[0].message }}
          </v-alert>
          <v-alert border="bottom" colored-border type="success" v-else>
            {{ data.notificationResume[0].message }}
          </v-alert>
        </template>
      </v-card-text>
      <v-card-text>
        <History :resumeData="data.notificationResume" />
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import History from './History';
export default {
  components:{
    History
  },
  props: {
    data: {
      type: Object,
      default: {
        key: "",
        name: "",
        description: "",
        notificationResume: [],
      }
    }
  },
}
</script>

<style lang="scss">
.card-container{
  margin: 30px 0;
}  
</style>