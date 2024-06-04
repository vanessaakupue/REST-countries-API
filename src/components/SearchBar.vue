<template>
    <div>
        <div class=" position-relative px-4 py-2 shadow-bottom" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'" style="border-radius: 4px;">
            <svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" class="position-absolute " style="top: 40%;"><path fill="currentColor" d="M15.5 14h-.79l-.28-.27A6.471 6.471 0 0 0 16 9.5A6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5S14 7.01 14 9.5S11.99 14 9.5 14"/></svg>
            <input type="search"  name="" id="" placeholder="Search for a country" class="mx-4" style="border: none; background-color: transparent; outline: none;" >
        </div>
    </div>
</template>
<script>

export default {
    props: {
        theme: String
    }
}

</script>
<style>

</style>