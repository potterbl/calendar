<template>
  <div class="dialog" @click="closeDialog">
    <div class="dialog__message" @click.stop>
      <div class="dialog__header">
        <p>{{ currentMonth }} {{ day.day }}, {{currentYear}} р.</p>
      </div>
      <div class="dialog__body" v-if="day.eventMonth != '' && day.eventMonth == currentMonth && day.eventYear == currentYear">
        <p>{{ day.eventTime }} по Києву</p>
        <p>{{day.eventTimeLond}} по Лондону</p>
        <p>{{ day.event }}</p>
      </div>
      <div class="dialog__body" v-else>
        <p>Немає жодної події!</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'dialog-content',
  props: ['day', 'currentMonth', 'currentYear'],
  data() {
    return {

    }
  },
  methods: {
    closeDialog(){
      this.$emit('handleDialog', false)
    }
  }
}
</script>

<style scoped>
.dialog{
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  background-color: rgba(0,0,0,0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.dialog__message{
  width: 400px;
  min-height: 200px;
  background-color: #fff;
  box-sizing: border-box;
  position: relative;
  cursor: auto;
}
.dialog__header{
  width: 100%;
  height: 40px;
  border-bottom: 1px solid black;
  display: flex;
  align-items: center;
  padding-left: 10px;
}
.dialog__body{
  width: 100%;
  min-height: 160px;
  text-align: center;
}
</style>