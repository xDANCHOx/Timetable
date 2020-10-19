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
            <div class="timetable__control-data"><button @click="$refs.vuecal.previous()">&lt;</button>{{interval.startData}} - {{interval.endData}}<button @click="$refs.vuecal.next()">&gt;</button></div>
            <div class="d-flex-center">
              <div class="timetable__control__active-view" v-for="(view, index) in views" :key="view.id">
                <button @click="selectedView(view.title, index)" :class="{'active-view': view.active}">{{view.title}}</button>
              </div>
            </div>
          </div>
          <div class="timetable__filters">
            <div>
              <select class="timetable__filters-select">
                <option value="" disabled selected hidden>Course level</option>
                <option :value="level.value" v-for="level in courseLevels" :key="level.id">
                  {{level.title}}
                </option>
              </select>
              <select class="timetable__filters-select">
                <option :value="group.value" v-for="group in groups" :key="group.id">
                  {{group.title}}
                </option>
              </select>
              <select class="timetable__filters-select">
                <option :value="item.value" v-for="item in classes" :key="item.id">
                  {{item.title}}
                </option>
              </select>
            </div>
            <div>
              <select class="timetable__filters-select" style="width: auto">
                <option :value="timezone.value" v-for="timezone in timezones" :key="timezone.id">
                  {{timezone.title}}
                </option>
              </select>
            </div>
          </div>
          <vue-cal 
          small
          :active-view.sync="activeView"
          ref="vuecal"
          hide-view-selector
          hide-title-bar
          :events="events"
          editable-events
          @ready="getIntervalDate($event)"
          @view-change="getIntervalDate($event)"
          />
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import VueCal from 'vue-cal';
import 'vue-cal/dist/drag-and-drop.js'
import 'vue-cal/dist/vuecal.css';
export default {
  components: { VueCal },
  name: "App",
  data() {
    return {
      views: [
        {
          title: 'day',
          active: false
        },
        {
          title: 'week',
          active: true
        },
        {
          title: 'month',
          active: false
        },
      ],
      courseLevels: [
        {
          title: 'A',
          value: 'a'
        },
        {
          title: 'B',
          value: 'b'
        },
        {
          title: 'C',
          value: 'c'
        },
        {
          title: 'D',
          value: 'd'
        }
      ],
      groups: [
        {
          title: 'A2',
          value: 'a2'
        },
        {
          title: 'A1',
          value: 'a1'
        },
        {
          title: 'B2',
          value: 'b2'
        },
        {
          title: 'B1',
          value: 'b1'
        }
      ],
      classes: [
        {
          title: 'All classes',
          value: 'all'
        },
        {
          title: 'First',
          value: 'first'
        },
        {
          title: 'Second',
          value: 'second'
        },
        {
          title: 'Third',
          value: 'third'
        }
      ],
      timezones: [
        {
          title: '(UTC +04:00)  Asua/Yerevan',
          value: 'all'
        },
        {
          title: '(UTC +06:00)  Asua/Astana',
          value: 'first'
        },
        {
          title: '(UTC +08:00)  Asua/Pery',
          value: 'second'
        },
      ],
      events: [
        {
          start: '2020-10-19 14:00',
          end: '2020-10-19 17:30',
          title: 'Boring event',
        },
        {
          start: '2020-10-19 14:00',
          end: '2020-10-19 17:30',
          title: 'Boring event',
        },
        {
          start: '2020-10-19 14:00',
          end: '2020-10-19 17:30',
          title: 'Boring event',
        },
      ],
      activeView: 'week',
      interval:{
        startData: '',
        endData: '',
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
      this.interval.startData = data.startDate.toLocaleString('en-us', { day: 'numeric', month: 'long' });
      this.interval.endData = data.endDate.toLocaleString('en-us', { day: 'numeric', month: 'long' });
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
  background: #fbfbfb;
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
  position: relative;
}
.header__search {
  position: relative;
  left: 180px;
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
  padding: 20px 0 20px 40px;
  height: 100%;
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
  padding: 30px 25px;
  width: 100%;
  height: 100%;
}
.main-title {
  font-weight: 600;
  font-size: 32px;
  margin-bottom: 32px;
}
.timetable__control {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
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
  font-size: 18px
}
.timetable__control__active-view button {
  border:none;
  padding: 4px 12px;
  background: none;
  font-size: 16px;
  color: #383838;
  cursor: pointer;
  text-transform: capitalize;
  outline: none;
}
.timetable__control__active-view button:hover {
  background: #E1E5F9;
  border-radius: 4px;
}
.active-view {
  background: #E1E5F9 !important;
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
}
.timetable__filters-select {
  margin: 0px 8px;
  padding: 6px 12px;
  border: 1px solid #D9D9D9;
  background-color: white;
  border-radius: 4px;
  width: 178px;
  outline: none;
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
  border-right: 1px solid #D9D9D9;
}
.vuecal__flex > .vuecal__heading > .vuecal__flexlast-child {
  border: none;
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
.vuecal__time-column .vuecal__time-cell-line:before {
  border-top: 2px dashed #D9D9D9;
}
.vuecal__flex .vuecal__weekdays-headings {
  border:none;
}
</style>
