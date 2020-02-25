<template>
  <div class="resume">
    <div class="resume-bar" @mouseleave="showTooltip = false">
      <template v-if="resumeData.length > 0">
        <template v-for="(el, index) of resumeData">
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
    <transition name="resume-anim">
      <div class="resume-text" v-if="details !== null && showTooltip">
        <p>{{ details.message }}</p>
        <p>{{ humanDate }}</p>
      </div>
    </transition>
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
    }
  },
  computed: {
    humanDate() {
      if(this.details !== null){
        return this.$dateFns.format(this.details.createdAt,"dd-MM-yyyy hh:mm");
      }else{
        return "";
      } 
    }
  },
  beforeMount(){
    this.ramdomBoxes = Math.random();
  }
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

    .bar-segment {
      height: 50px;
      transition: ease .3s;
      cursor: pointer;

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
    top: 100%;
    left: 0;
    background: #fff;
    z-index: 100;
    width: 100%;
    border-radius: 5px;
    margin-top: 10px;
    box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
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