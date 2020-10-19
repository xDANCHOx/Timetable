<template>
  <div class="sidebar">
    <ul>
      <li>
        <img alt="Dashboard" src="../assets/icons/Dashboard.svg" />
        <p>Dashboard</p>
      </li>
      <li>
        <img alt="Groups" src="../assets/icons/Groups.svg" />
        <p>Groups</p>
      </li>
      <li class="active">
        <img alt="Timetable" src="../assets/icons/Timetable.svg" />
        <p>Timetable</p>
      </li>
      <li>
        <img alt="Course" src="../assets/icons/Course.svg" />
        <p>Course plans</p>
      </li>
      <li>
        <img alt="Users" src="../assets/icons/Users.svg" />
        <p>Users</p>
      </li>
      <li>
        <img alt="Roles" src="../assets/icons/Roles.svg" />
        <p>Roles</p>
      </li>
    </ul>
  </div>
</template>
<style scoped>
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
</style>
