<template>
  <div class="calendar">
    <div class="calendar__header">
      <h1>{{ currentMonth }} {{ currentYear }} р.</h1>
      <div class="buttons">
        <button type="button" class="btn btn-primary btn-custom" @click="prevMonth">&lt;</button>
        <button type="button" class="btn btn-primary btn-custom" @click="nextMonth">></button>
      </div>
    </div>
    <div class="calendar__body">
      <div class="body__header">
        <p>пн</p>
        <p>вт</p>
        <p>ср</p>
        <p>чт</p>
        <p>пт</p>
        <p>сб</p>
        <p>нд</p>
      </div>
      <hr>
      <div class="body__content">
        <div class="box__date clear" v-if="1 <= firstDayOfMonth"></div>
        <div class="box__date clear" v-if="2 <= firstDayOfMonth"></div>
        <div class="box__date clear" v-if="3 <= firstDayOfMonth"></div>
        <div class="box__date clear" v-if="4 <= firstDayOfMonth"></div>
        <div class="box__date clear" v-if="5 <= firstDayOfMonth"></div>
        <div class="box__date clear" v-if="6 <= firstDayOfMonth"></div>
        <div class="box__date" v-for="(day,index) in $store.state.days" :key="index"  :style="{backgroundColor: (day.eventMonth == currentMonth && day.eventYear == currentYear)? 'gray' : ''}"
        v-show="day.day <= maxDaysInMonth"
             @click="openEvent(day)"
        >
          <h2>{{ day.day }}</h2>
        </div>
      </div>
    </div>
  </div>
  <dialog-content v-if="showDialog" :currentYear="currentYear" :day="day" :currentMonth="currentMonth" @handleDialog="handleDialogEmit"/>
</template>

<script>
import DialogContent from "@/components/UI/dialog.vue";

export default {
  name: 'App',
  components: {DialogContent},
  data() {
    return {
      currentMonth: 'Березень',
      monthCounter: 0,
      currentYear: 2023,
      firstDayOfMonth: '',
      maxDaysInMonth: '',
      day: {},
      showDialog: false,
    }
  },
  methods: {
    handleDialogEmit(showDialog){
      this.showDialog = showDialog;
    },
    openEvent(day){
      this.day = day
      this.showDialog = !this.showDialog
    },
    nextMonth(){
      if(this.monthCounter == 11){
        this.monthCounter = 0
        this.currentYear++
        this.currentMonth = this.$store.state.months[this.monthCounter]
        this.updateDayOfWeek()
      } else {
        this.monthCounter++
        this.currentMonth = this.$store.state.months[this.monthCounter]
        this.updateDayOfWeek()
      }
    },
    prevMonth(){
      if(this.monthCounter == 0){
        this.monthCounter = 11
        this.currentYear--
        this.currentMonth = this.$store.state.months[this.monthCounter]
        this.updateDayOfWeek()
      } else {
        this.monthCounter--
        this.currentMonth = this.$store.state.months[this.monthCounter]
        this.updateDayOfWeek()
      }
    },
    updateDayOfWeek(){
      const month = this.monthCounter
      const firstDay = new Date(this.currentYear, this.monthCounter, 1).getDay()
      this.firstDayOfMonth = firstDay === 0 ? 7 : firstDay - 1

      const lastDay = new Date(this.currentYear, month + 1, 0).getDate()
      this.maxDaysInMonth = lastDay
      console.log(this.maxDaysInMonth)
    }
  },
  mounted() {
    const monthList = this.$store.state.months

    const date = new Date()
    const month = date.getMonth()

    this.currentMonth = monthList[month]
    this.monthCounter = month

    const firstDay = new Date(this.currentYear, month, 1).getDay()
    this.firstDayOfMonth = firstDay === 0 ? 7 : firstDay - 1

    const lastDay = new Date(this.currentYear, month + 1, 0).getDate()
    this.maxDaysInMonth = lastDay

    console.log(this.maxDaysInMonth)
  },
}
</script>

<style>
p{
  margin: 0;
}
h1{
  margin: 0;
}
h2{
  margin: 0;
}
hr{
  margin-top: 0;

}
.calendar{
  width: 100%;
  height: 100%;
}
.calendar__header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.btn-custom{
  padding-left: 30px;
  padding-right: 30px;
}
.calendar__body{
  padding: 0 20px;
  box-sizing: border-box;
}
.body__header{
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}
.body__header p{
  width: 14.28%;
  text-align: center;
}
.body__content{
  width: 100%;
  height: 100%;
  display: flex;
  flex-wrap: wrap;
}
.box__date{
  width: 14.28%;
  height: 100px;
  box-sizing: border-box;
  border: 1px solid rgba(0,0,0,0.5);
  cursor: pointer;
}
.clear{
  border: 0;
}
</style>
