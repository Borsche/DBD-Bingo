<template>
    <div class="field">
        <img src="../assets/Border.png">
    </div>
</template>

<script>
    export default {
        name: 'BingoField',
        props: ['task']
    }
</script>

<style>
.field {
    height: 142px;
    width: 142px;
    box-shadow: inset 5px 5px 50px 10px rgba(0, 0, 0, 0.452);
    background-color: #583c11;
    background-image: url('../assets/LittleIcon.png');
    position:relative;
}

.field img {
    position: relativ;
    top: 10;
    left: 0;
}
</style>