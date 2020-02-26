<template>
  <div class="resume">
    <div class="resume-bar" @mouseleave="setTooltip(null)">
      <template v-if="resumeData.length > 0">
        <template v-for="(el, index) of resumeDataOrdered">
          <div :key="index" class="bar-segment" @mouseenter="setTooltip(el)"
            :class="{ 'error': el.isError, 'success' : !el.isError }"></div>
        </template>
        <template v-if="(limitBoxes - resumeData.length) > 0">
          <template v-for="(i, index) in (limitBoxes - resumeData.length)">
            <div class="no-data bar-segment" :key="ramdomBoxes + index"></div>
          </template>
        </template>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    resumeData: {
      type: Array,
      default: []
    },
    limitBoxes: {
      type: Number,
      default: 50
    },
  },
  data() {
    return {
      showTooltip: false,
      details: null,
      ramdomBoxes: 0,
    }
  },
  methods: {
    setTooltip(data) {
      this.details = data;
      this.showTooltip = true;
      this.$emit('eventData',data);
    },
  },
  computed: {
    humanDate() {
      if(this.details !== null){
        return this.$dateFns.format(this.details.createdAt,"dd-MM-yyyy hh:mm");
      }else{
        return "";
      } 
    },
    resumeDataOrdered(){
      return this.resumeData.sort((a, b) => (new Date(a.createdAt) > new Date(b.createdAt))?1:-1);
    }
  },
  mounted(){
    this.ramdomBoxes = Math.random();
  },
}
</script>

<style lang="scss">


.resume {
  position: relative;
  .resume-bar {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(10px, 1fr));
    width: 100%;
    gap: 3px;

    @include media('<=phone'){
      grid-template-columns: repeat(25, 1fr);
    }

    .bar-segment {
      height: 50px;
      transition: ease .2s;
      cursor: pointer;
      border-radius: 7px;

      &:hover{
        transform: translate(0, -10px);
      }

      &.error {
        background-color: $viva-red;
      }

      &.success {
        background-color: $viva-green;
      }
      &.no-data{
        background-color: $viva-grey-light;
      }
    }
  }
  .resume-text{
    padding: 10px;
    position: absolute;
    top: 0;
    left: 0;
    transform: translate(0, -120%);
    background: #fff;
    z-index: 100;
    width: 100%;
    border-radius: 3px;
    //margin-bottom: 40px;
    //box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
  }
}

.resume-anim{
  &-enter{
    transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    opacity: 0;
    transform: scale(.6);
  }
  &-enter-to{
    opacity: 1;
    transform: scale(1);
  }
  &-leave{
    transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    opacity: 1;
    transform: scale(1);
  }
  &-leave-to{
    opacity: 0;
    transform: scale(.6);
  }
}

</style>