<template>
  <div class="calendar">
    <div class="calendar-header">
      <i class="fa fa-fw fa-chevron-left" @click="subtractMonth"></i>
      <h4>{{month + ' - ' + year}}</h4>
      <i class="fa fa-fw fa-chevron-right" @click="addMonth"></i>
    </div>
    <ul class="weekdays">
      <li v-for="(day,day_index) in days" :key="day_index+'_day'">{{day}}</li>
    </ul>
    <ul class="dates">
      <li v-for="(blank,blank_index) in firstDayOfMonth" :key="blank_index+'_blank'">&nbsp;</li>
      <li
        v-for="(date,date_index) in daysInMonth"
        :key="date_index+'_date'"
        @click="getDate(date)"
        :class="{'current-day': date == initialDate &amp;&amp; month == initialMonth && year == initialYear}"
      >
        {{date}}
        <span></span>
      </li>
    </ul>
  </div>
</template>
<script>
import moment from "moment";
export default {
  name: "app",
  computed: {
    year: function() {
      var t = this;
      return t.dateContext.format("Y");
    },
    month: function() {
      var t = this;
      return t.dateContext.format("MMMM");
    },
    daysInMonth: function() {
      var t = this;
      return t.dateContext.daysInMonth();
    },
    currentDate: function() {
      var t = this;
      return t.dateContext.get("date");
    },
    firstDayOfMonth: function() {
      var t = this;
      var firstDay = moment(t.dateContext).subtract(t.currentDate - 1, "days");
      return firstDay.weekday();
    },
    initialDate: function() {
      var t = this;
      return t.today.get("date");
    },
    initialMonth: function() {
      var t = this;
      return t.today.format("MMMM");
    },
    initialYear: function() {
      var t = this;
      return t.today.format("Y");
    }
  },
  data() {
    return {
      today: moment(),
      dateContext: moment(),
      days: ["S", "M", "T", "W", "T", "F", "S"]
    };
  },
  methods: {
    addMonth: function() {
      var t = this;
      t.dateContext = moment(t.dateContext).add(1, "month");
    },
    subtractMonth: function() {
      var t = this;
      t.dateContext = moment(t.dateContext).subtract(1, "month");
    },
    getDate(date) {
      console.log(date)
    }
  }
};
</script>
<style scoped>
  .calendar {
    font-family: -apple-system,system-ui,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,sans-serif;
    border: 1px solid #eee;
    border-radius: 5px;
    width: 600px;
    padding: 15px 10px;
  }

  .calendar-header {
    display: flex;
  }

  .calendar-header>i:first-child {
    margin-right: auto;
    cursor: pointer;
  }

  .calendar-header>i:last-child {
    margin-left: auto;
    cursor: pointer;
  }

  li {
    list-style: none;
  }

  .weekdays {
    padding-left: 0;
    width: 100%;
    display: flex;
  }

  .weekdays li {
    text-align: center;
    width: 85px;
    font-weight: bold;
  }

  .dates {
    padding-left: 0;
    width: 100%;
    display: flex;
    flex-wrap: wrap;
  }

  .dates li {
    text-align: center;
    width: 85px;
    margin: 10px 0;
    cursor: pointer;
  }
</style>