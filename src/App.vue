<template>
  <div id="app">
    <div class="header">
      <div class="d-flex-center">
        <img alt="Logo" src="./assets/Logo.svg" />
        <div class="header__search">
          <img alt="search" src="./assets/Search.svg" />
          <input
            type="text"
            placeholder="Search"
            class="header__search-input"
          />
        </div>
      </div>
      <div class="header__profile d-flex-center">
        <img alt="Logo" src="./assets/Bell.svg" />
        <img alt="Logo" src="./assets/User.svg" style="margin: 0 8px 0 34px" />
        <p>Hanz Miller</p>
      </div>
    </div>
    <div class="layout">
      <div class="sidebar">
        <ul>
          <li>
            <img alt="Dashboard" src="./assets/Dashboard.svg" />
            <p>Dashboard</p>
          </li>
          <li>
            <img alt="Groups" src="./assets/Groups.svg" />
            <p>Groups</p>
          </li>
          <li class="active">
            <img alt="Timetable" src="./assets/Timetable.svg" />
            <p>Timetable</p>
          </li>
          <li>
            <img alt="Course" src="./assets/Course.svg" />
            <p>Course plans</p>
          </li>
          <li>
            <img alt="Users" src="./assets/Users.svg" />
            <p>Users</p>
          </li>
          <li>
            <img alt="Roles" src="./assets/Roles.svg" />
            <p>Roles</p>
          </li>
        </ul>
      </div>
      <div class="main">
        <h1 class="main-title">Timetable</h1>
        <div class="timetable">
          <div class="timetable__control">
            <div class="timetable__control-data">
              <button @click="$refs.vuecal.previous()">&lt;</button
              >{{ interval.startData }} - {{ interval.endData
              }}<button @click="$refs.vuecal.next()">&gt;</button>
            </div>
            <div class="d-flex-center">
              <div
                class="timetable__control__active-view"
                v-for="(view, index) in views"
                :key="view.id"
              >
                <button
                  @click="selectedView(view.title, index)"
                  :class="{ 'active-view': view.active }"
                >
                  {{ view.title }}
                </button>
              </div>
            </div>
          </div>
          <div class="timetable__filters">
            <div>
              <select class="timetable__filters-select" disabled>
                <option value="" disabled selected hidden>Course level</option>
                <option
                  :value="level.value"
                  v-for="level in courseLevels"
                  :key="level.id"
                >
                  {{ level.title }}
                </option>
              </select>
              <select class="timetable__filters-select" disabled>
                <option value="" disabled selected hidden>Group</option>
                <option
                  :value="group.value"
                  v-for="group in groups"
                  :key="group.id"
                >
                  {{ group.title }}
                </option>
              </select>
              <select class="timetable__filters-select" v-model="selectedClass">
                <option
                  :value="item.value"
                  v-for="item in classes"
                  :key="item.id"
                >
                  {{ item.title }}
                </option>
              </select>
            </div>
            <div>
              <select
                class="timetable__filters-select"
                style="width: auto"
                v-model="selectedTimezone"
              >
                <option
                  :value="timezone.value"
                  v-for="timezone in timezones"
                  :key="timezone.id"
                >
                  {{ timezone.title }}
                </option>
              </select>
            </div>
          </div>
          <vue-cal
            :time-from="5 * 60"
            :time-to="21 * 60"
            small
            :active-view.sync="activeView"
            ref="vuecal"
            hide-view-selector
            hide-title-bar
            :events="filteredClasses"
            editable-events
            @ready="getIntervalDate($event)"
            @view-change="getIntervalDate($event)"
            :time-cell-height="60"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueCal from "vue-cal";
import "vue-cal/dist/drag-and-drop.js";
import "vue-cal/dist/vuecal.css";
export default {
  components: { VueCal },
  name: "App",
  data() {
    return {
      views: [
        {
          title: "day",
          active: false
        },
        {
          title: "week",
          active: true
        },
        {
          title: "month",
          active: false
        }
      ],
      courseLevels: [
        {
          title: "A",
          value: "a"
        },
        {
          title: "B",
          value: "b"
        },
        {
          title: "C",
          value: "c"
        },
        {
          title: "D",
          value: "d"
        }
      ],
      groups: [
        {
          title: "A2",
          value: "a2"
        },
        {
          title: "A1",
          value: "a1"
        },
        {
          title: "B2",
          value: "b2"
        },
        {
          title: "B1",
          value: "b1"
        }
      ],
      classes: [
        {
          title: "All classes",
          value: "all"
        },
        {
          title: "First",
          value: "first"
        },
        {
          title: "Second",
          value: "second"
        }
      ],
      timezones: [
        {
          title: "(UTC +04:00) Asua/Yerevan",
          value: "04:00"
        },
        {
          title: "(UTC +03:00) Moscow, Saint Petersburg",
          value: "03:00"
        }
      ],
      events: [
        {
          start: "2020-10-19 9:00",
          end: "2020-10-19 12:00",
          title: "B2 – Kolette",
          class: "group-orange",
          value: "second"
        },
        {
          start: "2020-10-20 9:00",
          end: "2020-10-20 11:00",
          title: "A2 – Currywu",
          class: "group-violet",
          value: "second"
        },
        {
          start: "2020-10-20 13:00",
          end: "2020-10-20 16:00",
          title: "A1 – Kolette",
          class: "group-violet",
          value: "first"
        },
        {
          start: "2020-10-21 9:00",
          end: "2020-10-21 10:00",
          title: "A2 – Currywu",
          class: "group-violet",
          value: "second"
        },
        {
          start: "2020-10-22 07:00",
          end: "2020-10-22 10:00",
          title: "A1 – Kolette",
          class: "group-orange",
          value: "first"
        }
      ],
      activeView: "week",
      interval: {
        startData: "",
        endData: ""
      },
      selectedClass: "all",
      selectedTimezone: "04:00"
    };
  },
  computed: {
    filteredClasses() {
      if (this.selectedClass === "all") {
        return this.events;
      } else {
        return this.events.filter(item => item.value === this.selectedClass);
      }
    }
  },
  methods: {
    selectedView(title, i) {
      this.activeView = title;
      this.views.forEach((view, index) => {
        if (index === i) {
          view.active = true;
        } else {
          view.active = false;
        }
      });
    },
    getIntervalDate(data) {
      this.interval.startData = data.startDate.toLocaleString("en-us", {
        day: "numeric",
        month: "long"
      });
      this.interval.endData = data.endDate.toLocaleString("en-us", {
        day: "numeric",
        month: "long"
      });
    }
  }
};
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #272848;
  width: 100%;
  height: 100%;
}
*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
  font-family: "Rubik", sans-serif;
}
html,
body {
  width: 100%;
  height: 100%;
  background-color: #f7f7fd;
  font-size: 18px;
}
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}
.d-flex-center {
  display: flex;
  align-items: center;
}
.layout {
  display: flex;
  height: 100%;
  width: 100%;
}
.header {
  background-color: white;
  border-bottom: 1px solid #e1e5f9;
  height: 60px;
  padding: 10px 34px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  z-index: 100;
  position: fixed;
  width: 100%;
}
.header__search {
  position: relative;
  left: 186px;
  display: flex;
  align-items: center;
}
.header__search-input {
  border: none;
  font-size: 18px;
  margin-left: 12px;
}
.sidebar {
  box-shadow: 3px 3px 30px rgba(0, 0, 0, 0.1);
  background: white;
  width: 252px;
  padding: 28px 0 20px 40px;
  height: 100%;
  margin-top: 60px;
  position: fixed;
}
.sidebar li {
  display: flex;
  align-items: center;
  margin: 24px 0px;
  width: 100%;
  color: #72849e;
}
.sidebar li p {
  margin-left: 9px;
}
.active {
  color: #258ffb !important;
  border-right: 4px solid #258ffb;
}
.main {
  padding: 40px 25px;
  width: 100%;
  height: 100%;
  margin-top: 60px;
  margin-left: 250px;
}
.main-title {
  font-weight: 600;
  font-size: 32px;
  margin-bottom: 32px;
  margin-left: 54px;
}
.timetable__control {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
  margin-left: 54px;
}
.timetable__control-data {
  color: #383838;
  display: flex;
  align-items: center;
}
.timetable__control-data button {
  border: none;
  outline: none;
  background: none;
  cursor: pointer;
  padding: 0 16px;
  font-size: 18px;
}
.timetable__control__active-view button {
  border: none;
  padding: 4px 12px;
  background: none;
  font-size: 16px;
  color: #383838;
  cursor: pointer;
  text-transform: capitalize;
  outline: none;
}
.timetable__control__active-view button:hover {
  background: #e1e5f9;
  border-radius: 4px;
}
.active-view {
  background: #e1e5f9 !important;
  border-radius: 4px;
}
.timetable__filters {
  background: white;
  border-radius: 4px;
  padding: 14px 24px;
  margin-bottom: 36px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-left: 54px;
}
.timetable__filters-select {
  margin: 0px 8px;
  padding: 6px 12px;
  border: 1px solid #d9d9d9;
  background-color: white;
  border-radius: 4px;
  width: 178px;
  outline: none;
  font-size: 16px;
}
.vuecal__cell:before {
  border: none;
}
.vuecal__cells {
  background: white;
  border-radius: 8px;
}
.vuecal__heading > .vuecal__flex {
  height: auto;
  border-right: 1px solid #d9d9d9;
}
.vuecal {
  box-shadow: none;
}
.vuecal__time-column .vuecal__time-cell-line:before:first-child {
  border: none;
}
.vuecal__heading:last-child > .vuecal__flex {
  border: none;
}
.vuecal__time-cell:first-child > .vuecal__time-cell-line {
  display: none;
}
.vuecal__time-cell:first-child > .vuecal__time-cell-label {
  display: none;
}
.vuecal__time-column .vuecal__time-cell-line:before {
  border-top: 2px dashed #d9d9d9;
  left: 55px;
}
.vuecal__flex .vuecal__weekdays-headings {
  border: none;
}
.vuecal__cell--current,
.vuecal__cell--today {
  background: rgba(37, 143, 251, 0.1);
  border-radius: 8px;
}
.vuecal__cell {
  padding: 0 8px;
}
.vuecal__time-cell {
  /* height: 60px !important; */
  text-align: left !important;
}
.vuecal__time-cell-label {
  position: relative;
  bottom: 8px;
  color: #383838;
}
.vuecal__flex .weekday-label {
  color: #72849e;
}
.vuecal__event.group-orange {
  background-color: #ffeece;
  border-radius: 4px;
  border-left: 8px solid #ffbc42;
}
.vuecal__event.group-violet {
  background-color: #ccc6ff;
  border-radius: 4px;
  border-left: 6px solid#A499FF;
}
.vuecal__no-event {
  display: none;
}
.vuecal--month-view .vuecal__cell {
  height: 80px;
}
.vuecal--month-view {
  margin-left: 54px;
}
</style>
