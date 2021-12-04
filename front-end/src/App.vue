<template>
<div id="app">
  <div class="header">
    <router-link to="/">
    </router-link>
    <div class="title">
      <h1>CS 260 Assignments</h1>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">

    <router-link to="/">Grades</router-link>
    <br>
    <router-link to="/submit">Submit</router-link>
    <br>
    <router-link to="/admin">Admin</router-link>

  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #F4FFF6;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: #FF6A4A;
  color: #1C454F;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #B6E5FF;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
