<template>
    <div class="message-container">
       <p> {{msg}} </p>
    </div>    
</template>

<script>
export default {
    name: "Message",
    props: {
        msg: String
    }
}
</script>

<style>
    .message-container {
        color: white;
        background: rgb(8, 167, 179);
        border: 2px solid rgb(189, 185, 185);
        border-radius: 5px;
        padding: 10px;
        max-width: 450px;
        margin-bottom: 10px;
        margin-left: 625px;
    }
</style>