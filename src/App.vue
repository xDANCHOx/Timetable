<template>
  <div id="app">
    <Header />
    <div class="layout">
      <Sidebar />
      <Timetable />
    </div>
  </div>
</template>

<script>
import Timetable from "./components/Timetable";
import Sidebar from "./components/Sidebar";
import Header from "./components/Header";
export default {
  components: { Timetable, Sidebar, Header },
  name: "App"
};
</script>
