<template>
    <span class="square" :class="green > 0 ? color(green) : ''">
        
    </span>
</template>
<script>
export default {
    props: ['green', 'currentColor'],
    methods: {
       color (currentColor) {
           /* ["red", "yellow", "blue", "green", "purple", "orange" ,"#fff"] */
           switch (currentColor) {
                case 0:
                   return 'red' 
                case 1:
                   return 'yellow'
                case 2:
                    return 'blue'
                case 3:
                    return 'green'
                case 4:
                    return 'purple'
                case 5:
                    return 'orange'
                case 6:
                    return 'white'
           }
       }
    }
}
</script>
<style>
* {
    margin: 0;
    padding: 0;
}

/* ["red", "yellow", "blue", "green", "purple", "orange" ,"#fff"] */
.red.square {
    color:red
}

.yellow.square {
    background-color:yellow;
}

.blue.square {
    background-color: blue;
}

.green.square {
    background-color: green;
}

.purple.square {
    background-color: purple;
}

.orange.square {
    background-color: orange;
}

.white.square {
    background-color: white;
}

.square {
    width: 20px;
    height: 20px;
    display: inline-block;
    background-color: grey;
    border: 1px solid orangered;
}




</style>
