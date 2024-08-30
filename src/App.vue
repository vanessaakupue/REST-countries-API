<template>
  <div :class="theme === 'light' ? 'light-theme' : 'dark-theme' " class="" style=" position: relative; max-width: 2000px;">
     <header class="fixed-header d-flex align-items-center justify-content-between px-5 shadow-bottom" style="height: 60px;" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el' ">
       <span class="fw-6" style="font-size: 18px;" @click="goToHome" ><a class="fw-6" style="text-decoration:none; font-size: 18px; color: inherit;" href="#"> Countries of the world </a></span>
       <div class="d-flex align-items-center gap-2 float-right" style="cursor: pointer; font-size: 12px;" @click="changeTheme">
           <svg xmlns="http://www.w3.org/2000/svg" width="11" height="11" viewBox="0 0 14 14"><path fill="white" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" d="M12 10.48A6.55 6.55 0 0 1 6.46.5a6.55 6.55 0 0 0 1 13A6.46 6.46 0 0 0 13 10.39a6.79 6.79 0 0 1-1 .09Z"/></svg>
           {{ theme === 'dark' ? 'Light Mode' : 'Dark Mode' }}
       </div>

     </header>

     <main class="px-5">

      <RouterView :theme="theme" />
 
     </main>

     <footer>
      <div :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el' " class="d-flex align-items-center justify-content-between px-5 py-3 shadow-bottom" >
        <div style="font-size: 13px;" >Created by Vanessa Akupue</div>
        <div>
          <a href="https://www.linkedin.com/in/fumnanya-akupue-72375229b/" target="_blank"  style="text-decoration: none; color: inherit;" >
            <svg class="about-item" xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24"><path fill="currentColor" fill-rule="evenodd" d="M1 2.838A1.838 1.838 0 0 1 2.838 1H21.16A1.837 1.837 0 0 1 23 2.838V21.16A1.838 1.838 0 0 1 21.161 23H2.838A1.838 1.838 0 0 1 1 21.161zm8.708 6.55h2.979v1.496c.43-.86 1.53-1.634 3.183-1.634c3.169 0 3.92 1.713 3.92 4.856v5.822h-3.207v-5.106c0-1.79-.43-2.8-1.522-2.8c-1.515 0-2.145 1.089-2.145 2.8v5.106H9.708zm-5.5 10.403h3.208V9.25H4.208zM7.875 5.812a2.063 2.063 0 1 1-4.125 0a2.063 2.063 0 0 1 4.125 0" clip-rule="evenodd"/></svg>
          </a>
        </div>
      </div>
     </footer>

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
    goToHome() {
      this.$router.push({ name: 'home'})
    },
     changeTheme() {
       this.theme = this.theme === "dark" ? "light" : "dark";
       localStorage.setItem("theme", this.theme);
     }
   },
   mounted() {
     const savedTheme = localStorage.getItem("theme");
     if (savedTheme) {
       this.theme = savedTheme;
     }
   }
 }
 
 
 </script>
 
 <style>
 .fixed-header {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1000;
  width: 100%;
  max-width: 2000px;
 }
 .linkedin:hover {
  color: red;
 }

 </style>
