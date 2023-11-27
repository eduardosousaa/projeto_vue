<template>
    <div id="main-banner">
        <h1>Make Your Burger</h1>
    </div>
</template>

<script>

    export default {
        name: 'Banner'
    }

</script>

<style scoped>

    #main-banner {
        background-image: url('../../public/img/burger.jpg');
        background-position: 0 -250px;
        background-size: cover;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        height: 500px;
    }

    #main-banner h1 {
        color: #fff;
        background-color: #222;
        font-size: 60px;
        text-align: center;
        padding: 20px 40px;
    }

</style>