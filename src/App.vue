<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>
<!--main page header and content organisation-->
<style>
  @import url("https://use.typekit.net/bfl6rmj.css");
</style>
<style>
  @import url("https://use.typekit.net/bfl6rmj.css");
</style>

<template>
  <header>
    <nav>
      <div class="header">
  <a href="#default" class="logo">Women of Film and Sound History</a>
  <div class="header-right">
    <RouterLink to="/">Home</RouterLink>
  </div>
</div>
      
        
      </nav>
  </header>
  <div class="content">
<router-view />
</div>
</template>

<style scoped>


.header {
  overflow: hidden;
  background-color:  #C0C6B8;
  padding: 20px 10px;
  z-index : 1;
  position: absolute;
  
}

/* Style the header links */
.header a {
  color: black;
  text-align: center;
  padding: 12px;
  text-decoration: none;
  font-size: 18px;
  line-height: 25px;
  border-radius: 4px;
  font-family: "mundial", sans-serif;
font-weight: 400;
font-style: normal;
}

/* Style the logo link (notice that we set the same value of line-height and font-size to prevent the header to increase when the font gets bigger */
.header a.logo {
  font-size: 25px;
  
  font-family: "gambado-sans-forte", sans-serif;
font-weight: 700;
font-style: normal;

}

/* Change the background color on mouse-over */
.header a:hover {
  background-color: #ddd;
  color: black;
}

/* Style the active/current link*/
.header a.active {
  background-color: dodgerblue;
  color: white;
}

/* Float the link section to the right */
.header-right {
  float: right;
}

/* Add media queries for responsiveness - when the screen is 500px wide or less, stack the links on top of each other */
@media screen and (max-width: 500px) {
  .header a  {
    float: none;
    
    font-size: 10px;
    
  }
  .header-right {
  float: none;
}
  .header a.logo {
  font-size: 15px;
  font-weight: bold;
}
}

</style>
