<template>
  <div class="card-container" v-if="data.notificationResume.length > 0">
    <v-card>
      <v-card-text>
        <h2>{{ data.name }}</h2>
        <p><small class="caption">{{ data.description }}</small></p>
        <template v-if="data.notificationResume.length > 0">
          <v-alert border="bottom" colored-border :type="alertType">
            {{ (alternativeData !== null)?alternativeData.message:data.notificationResume[data.notificationResume.length - 1].message }} <small>{{ humanDate }}</small>
          </v-alert>
        </template>
      </v-card-text>
      <v-card-text>
        <History :resumeData="data.notificationResume" @eventData="alternativeInfo"/>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import History from './History';
export default {
  components: {
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
  data() {
    return {
      alternativeData: null
    }
  },
  methods: {
    alternativeInfo(data) {
      this.alternativeData = data;
    }
  },
  computed: {
    alertType() {
      if (this.alternativeData !== null) {
        return (this.alternativeData.isError) ? "error" : "success";
      } else {
        return (this.data.notificationResume[this.data.notificationResume.length - 1].isError) ? "error" : "success";
      }
    },
    humanDate() {
      if (this.alternativeData !== null) {
        return this.$dateFns.format(this.alternativeData.createdAt, "dd-MM-yyyy hh:mm");
      } else {
        return this.$dateFns.format(this.data.notificationResume[this.data.notificationResume.length - 1].createdAt, "dd-MM-yyyy hh:mm");
      }
    },
  },
}
</script>

<style lang="scss">
.card-container{
  margin: 30px 0;
}  
.v-card__title{
  text-transform: capitalize;
}
</style>