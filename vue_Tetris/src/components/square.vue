<template>
    <span class="square" :class="green > 0 ? color(green) : ''">
      <!--   <p>{{green}}</p> -->
    </span>
</template>
<script>
export default {
    props: ['green', 'currentColor'],
    methods: {
       color (currentColor) {
           /* ["red", "yellow", "blue", "green", "purple", "orange" ,"#fff"] */
           switch (currentColor) {
                case 1:
                   return 'red' 
                case 2:
                   return 'yellow'
                case 3:
                    return 'blue'
                case 4:
                    return 'green'
                case 5:
                    return 'purple'
                case 6:
                    return 'orange'
                case 7:
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
    background-color:red
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
