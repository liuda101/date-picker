<template>
<div class="date-picker">
  <div class="date-picker-h">
    <button>Left</button>
    <h2>七月 2017年</h2>
    <button>right</button>
  </div>

  <div class="date-picker-c">
    <div class="date-picker-c-h">
      <span v-for="week in weeks">周{{week}}</span>
    </div>

    <div class="date-picker-panel">
      <a v-for="date in dates" :class="{current: date.currentMonth, today: date.today, selected: date.selected}" @click="onDateSelected(date)">{{date.text}}</a>
    </div>
  </div>
</div>
</template>

<script>
const dayLong = 24 * 60 * 60 * 1000
export default {
  name: 'date-picker',
  data () {
    return {
      weeks: ['1', '2', '3', '4', '5', '6', '7'],

      dates: []
    }
  },
  props: {
    selected: {
      type: Date,
      default: null
    }
  },
  methods: {
    calDates: function () {
      var now = new Date()
      // var nowTime = now.getTime()

      var nowWeek = now.getDay()
      var lastMonthDayCount = 0
      if (nowWeek === 0) {
        lastMonthDayCount = 6
      } else {
        lastMonthDayCount = nowWeek - 1
      }

      // var lastMonth = this.getLastMonth(now)
      // 上个月
      var firstDate = new Date()
      firstDate.setDate(1)
      for (let i = 0; i < lastMonthDayCount; i++) {
        let date = new Date(firstDate.getTime() - dayLong * (i + 1))
        // debugger
        this.dates.splice(0, 0, {
          text: date.getDate(),
          currentMonth: false,
          today: false,
          selected: this.isSameDay(this.selected, date),
          value: date.getTime()
        })
      }

      // 当前月
      var nextMonthFirstDate = new Date()
      nextMonthFirstDate.setDate(1)
      var currentMonth = now.getMonth()
      var currentYear = now.getFullYear()
      currentMonth++
      if (currentMonth > 11) {
        currentMonth = 0
        currentYear++
      }
      nextMonthFirstDate.setFullYear(currentYear)
      nextMonthFirstDate.setMonth(currentMonth)
      var currentLastDate = new Date(nextMonthFirstDate - dayLong)
      for (let i = 0; i < currentLastDate.getDate(); i++) {
        let d = new Date(firstDate.getTime() + dayLong * i)
        this.dates.push({
          text: d.getDate(),
          currentMonth: true,
          today: this.isSameDay(now, d),
          selected: this.isSameDay(this.selected, d),
          value: d.getTime()
        })
      }

      var deltaCount = 42 - this.dates.length
      for (let i = 0; i < deltaCount; i++) {
        let d = new Date(nextMonthFirstDate.getTime() + dayLong * i)
        this.dates.push({
          text: d.getDate(),
          currentMonth: false,
          today: false,
          selected: this.isSameDay(this.selected, d),
          value: d.getTime()
        })
      }
    },
    isSameDay: function (d1, d2) {
      return d1.getFullYear() === d2.getFullYear() && d1.getMonth() === d2.getMonth() && d1.getDate() === d2.getDate()
    },
    isLeapYear: function (year) {
      if (year % 100 === 0) {
        if (year % 400 === 0) {
          return true
        } else {
          return false
        }
      } else if (year % 4 === 0) {
        return true
      } else {
        return false
      }
    },

    onDateSelected (date) {
      this.$emit('selected', date.value)
    }
  },
  mounted () {
    this.calDates()
  }
}
</script>

<style>
.date-picker-panel{
  width: 420px;
}
.date-picker-panel a{
  display: inline-block;
  width: 60px;
  text-align: center;
  height: 40px;
  line-height: 40px;
  border-radius: 5px;
  cursor: pointer;
}
.date-picker-panel a{
  color: gray;
}
.date-picker-panel a.current {
  color: blue;
}
.date-picker-panel a.today {
  background: black;
  color: white;
}
.date-picker-panel a.selected {
  background: orange;
  color: white;
}
</style>
