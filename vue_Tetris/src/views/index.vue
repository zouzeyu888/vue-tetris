<template>
  <div>
    <div v-for="i in 20" :key="i" class="row">
      <square v-for="j in 10" :key="j" :green="board[i - 1][j - 1]"></square>
    </div>
    <div id="box">
        <button @click="left" id="left">左移</button>
        <button @click="changeShape" id="change">变形</button>
        <button @click="rotateShape" id="rotateShape">旋转</button>
    </div>
    
  </div>
</template>
<script>
import square from '../components/square.vue'
export default {
    components: {
        square
    },
    data () {
        return {
            board: Array(20),
            current: 0,
            currentRow: 0,
            currentCol: 0,
            shapes: [ /* O I L Z S J T */
                    /* shapeO */
                    ['CC00', 'CC00', 'CC00', 'CC00'],
                    /* shapeI */
                    ['8888', 'F000', '8888', 'F000'],
                   /*  spapeL */
                    ['88C0', '2E00', 'C440', 'E800'],
                    /* shapeZ */
                    ['C600', '4C80', 'C600', '4C80'],
                   /*  shapeS */
                    ['6C00', '8C40', '6C00', '8C40'],
                    /* shapeT */
                    ['E400', '8C80', '4E00', '4C40'],
                    /* shapeJ */
                    ['44C0', 'E200', 'C880', '8E00']
                    ] 

        }
    },
    // 生命周期 钩子
    created () {
        for (let i = 0; i < 20; i++) {
            let row = []
            // eslint-disable-next-line keyword-spacing
            for(let j = 0; j < 10; j++) {
                row.push(0)
            }
            this.board[i] = row
        }
        /* this.board[0][0] = 1
        this.board[1][0] = 1
        this.board[1][1] = 1
        this.board[1][2] = 1
        console.log(this.board) */
    },
    methods: {
        left () {
            this.draw(0)
            this.current++
            if (this.current > 7) {
                this.current = 0
            }
            this.draw(1)
        },
        // 组件化 响应式
        draw (type) {
            this.board[0][this.current] = type
            this.board[1][this.current] = type
            this.board[1][this.current + 1] = type
            this.board[1][this.current + 2] = type
            this.board = this.board.slice()
        },

    

        // 画一个形状
        drawShape (shape) {
            for (let i = 0; i < 4; i++) {
                let binaryStr = parseInt(shape[i], 16).toString(2)
                if (binaryStr.length < 4) {
                    binaryStr = Array(4 - binaryStr.length).fill(0).join('') + binaryStr
                }
                for (let j = 0; j < binaryStr.length; j++) {
                    this.board[i][j] = parseInt(binaryStr[j]) 
                    this.board = this.board.slice()
                }
            }
            /* console.log(this.board) */
        },

        // 下一个图形
        changeShape () {
            this.currentCol = 0
            this.currentRow = (this.currentRow + 1) % this.shapes.length
            let currentShape = this.shapes[this.currentRow][this.currentCol]
            this.drawShape(currentShape)       
            console.log(currentShape, 1)
        },
        // 旋转
        rotateShape () {
            this.currentCol = (this.currentCol + 1) % this.shapes[this.currentRow].length
            let currentShape = this.shapes[this.currentRow][this.currentCol]
            this.drawShape(currentShape)
            console.log(currentShape, 2)
        }

    }
}
</script>
<style>
.row {
  height: 20px;
}

#box {
    position: absolute;
    width: 200px;
    height: 100px;
    border: 1px solid black;
    top: 200px;
    right: 300px;
    
}
/* 左移 */
#left {
    position: absolute;
    left: 10px;
}

/* 变形 */
#change {
    position: absolute;
    left: 50px;
}

/* 旋转图形 */
#rotateShape {
    position: absolute;
    left: 90px;
}
</style>