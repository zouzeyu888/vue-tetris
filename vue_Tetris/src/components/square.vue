<template>
    <span class="square" :class="green == 1 ? 'green' : ''">

    </span>
</template>
<script>
export default {
    props: ['green']
}
</script>
<style>
* {
    margin: 0;
    padding: 0;
}
.green.square {
    background-color: green;
}
.square {
    width: 20px;
    height: 20px;
    display: inline-block;
    background-color: red;
    border: 1px solid orangered;
}




</style>
