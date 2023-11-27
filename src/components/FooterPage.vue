<template>
    <footer id="footerPage">
        <p>Make Your Burguer &copy; 2023</p>
    </footer>
</template>

<script>
    export default {
        name: 'FooterPage'
    }

</script>

<style scoped>

    #footerPage {
        height: 200px;
        background-color: #222;
        color: #FCBA03;
        border-top: 4px solid #111;
        display: flex;
        align-items: center;
        justify-items: center;
        width: 100%;
    }

    p {
        margin: auto;
    }

</style>