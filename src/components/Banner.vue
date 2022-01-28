<template>
    <div id="main-banner">
        <h1>Monte sua pizza</h1>
    </div>
</template>

<script>
export default {
    name: "Banner"
}
</script>

<style>
    #main-banner {
        background-image: url('/img/imagem.jpg');
        background-position: 0 -200px;
        background-size: cover;
        height: 500px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
    }

    #main-banner h1 {
        color: white;
        text-align: center;
        font-size: 50px;
        background-color: #222;
        padding: 20px 40px;
    }

</style>