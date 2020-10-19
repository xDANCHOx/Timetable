<template>
  <div class="header">
    <div class="d-flex-center">
      <img alt="Logo" src="../assets/icons/Logo.svg" />
      <div class="header__search">
        <img alt="search" src="../assets/icons/Search.svg" />
        <input type="text" placeholder="Search" class="header__search-input" />
      </div>
    </div>
    <div class="header__profile d-flex-center">
      <img alt="Logo" src="../assets/icons/Bell.svg" />
      <img
        alt="Logo"
        src="../assets/icons/User.svg"
        style="margin: 0 8px 0 34px"
      />
      <p>Hanz Miller</p>
    </div>
  </div>
</template>
<style scoped>
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
</style>
