<template>
  <div :class="theme === 'light' ? 'light-theme' : 'dark-theme' " class="" style="max-width: 1700px;">
     <header class="d-flex align-items-center justify-content-between px-5 shadow-bottom" style="height: 60px;" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el' ">
       <span class="fw-6" style="font-size: 18px;">Where in the world?</span>
       <div class="d-flex align-items-center gap-2 float-right" style="cursor: pointer; font-size: 12px;" @click="changeTheme">
           <svg xmlns="http://www.w3.org/2000/svg" width="11" height="11" viewBox="0 0 14 14"><path fill="white" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" d="M12 10.48A6.55 6.55 0 0 1 6.46.5a6.55 6.55 0 0 0 1 13A6.46 6.46 0 0 0 13 10.39a6.79 6.79 0 0 1-1 .09Z"/></svg>
           {{ theme === 'dark' ? 'Light Mode' : 'Dark Mode' }}
       </div>

     </header>

     <main class="px-5">

    <!-- <HomeView /> -->
       <RouterView :theme="theme" />
 
     </main>

  </div>
 </template>

 <script>
import { RouterView } from 'vue-router';

 
 export default {
   name: "App",
   components: {
     RouterView,
   },
   data() {
     return {
       theme: localStorage.getItem("theme") || "light",
     }
   },
   methods: {
     changeTheme() {
       // console.log('I am changing the theme')
       this.theme = this.theme === "dark" ? "light" : "dark";
       localStorage.setItem("theme", this.theme);
     }
   },
   mounted() {
     const savedTheme = localStorage.getItem("theme");
     if (savedTheme) {
       this.theme = savedTheme;
     }
     console.log('Theme:', this.theme);
   }
 }
 
 
 </script>
 
 <style>
 

 </style>
