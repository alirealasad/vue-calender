<template>
  <div id="app">
    <div class="calendar">
      <div class="calendar-header">
        <div class="year-picker">
          <span class="change" @click="setMonth(month-1)" id="prev-year">
            <pre>&lt;</pre>
          </span>
          <span id="month" @click="showMonths = true">{{monthNames[month]}}</span>
          <span class="change" @click="setMonth(month+1)" id="next-year">
            <pre>&gt;</pre>
          </span>
        </div>
        <div class="year-picker">
          <span class="change" @click="setYear(year-1)" id="prev-year">
            <pre>&lt;</pre>
          </span>
          <span id="year">{{year}}</span>
          <span class="change" @click="setYear(year+1)" id="next-year">
            <pre>&gt;</pre>
          </span>
        </div>
      </div>
      <div class="calendar-body">
        <div class="calendar-week-day">
          <div>Sun</div>
          <div>Mon</div>
          <div>Tue</div>
          <div>Wed</div>
          <div>Thu</div>
          <div>Fri</div>
          <div>Sat</div>
        </div>
        <div class="calendar-days">
          <div v-for="n in daysOfMonth(year)[month]+getFirstDay()" :key="n" class="calendar-day-hover" :class="{'curr-date':n-getFirstDay() === currDate.date() && year === currDate.year() && month === currDate.month()}">
            <template v-if="n > getFirstDay()">
              {{n-getFirstDay()}}
              <span></span>
              <span></span>
              <span></span>
              <span></span>
            </template>
          </div>

        </div>
      </div>
      <div class="calendar-footer">
        <div class="toggle">
          <span>Dark Mode</span>
          <div class="dark-mode-switch" @click="toggleDarkMode()">
            <div class="dark-mode-switch-ident"></div>
          </div>
        </div>
      </div>
      <div class="month-list" :class="{'show':showMonths}">
        <div v-for="(month, index) in monthNames" :key="month">
          <div @click="setMonth(index)">
            {{month}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  methods: {
    getFebDays(year) {
      return moment(year.toString() + "-02", "YYYY-MM").daysInMonth();
    },
    daysOfMonth(year) {
      return [31, this.getFebDays(year), 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
    },
    setYear(value){
      this.year = value
    },
    setMonth(value){
      if (typeof value === "number" && value <= 11 && value >= 0  ) {
        this.month = value
      }
      if (value === -1) {
        this.setYear(this.year-1)
        this.month = 11
      }
      if (value === 12) {
        this.setYear(this.year+1)
        this.month = 0
      }
      if (this.showMonths) {
        this.showMonths = false
      }
    },
    getFirstDay(){
      return moment(this.year.toString()+"-0"+(this.month+1).toString()+"-01").day();
    },
    toggleDarkMode(){
      let docClasess = document.body.classList;
        docClasess.toggle("dark");
        docClasess.toggle("light");
        this.dark = false
    }
  },
  data() {
    return {
      monthNames: moment.months(),
      currDate:moment(),
      year:moment().year(),
      month:moment().month(),
      showMonths:false
    };
  },
};
</script>
