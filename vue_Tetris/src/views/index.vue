<template>
  <div>
    <div v-for="i in 20" :key="i" class="row">
      <square v-for="j in 10" :key="j" :green="board[i - 1][j - 1].color" :currentColor="currentRow"></square>
    </div>
    <div id="box">
        <button @click="left" id="left" @keyup.left="left">左移</button>
        <button @click="right" id="right" @keyup.right="right">右移</button>
        <button @click="down" id="down">下移</button>
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
            currentX: 0,
            currentY: 0,
            nextShape: parseInt(Math.random(0, 1) * 8),
            currentRow: 1,
            currentCol: 0,
            currentKeyCode: 0,
            shapes: [ /* O I L Z S J T */
                    /* shapeO */
                    [],
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
                row.push({
                    status: 0,
                    color: 0
                })
            }
            this.board[i] = row
        }
        let _that = this
        document.onkeydown = function (e) {
            let key = window.event.keyCode
            if (key === 37) {
                _that.left()
            }
            if (key === 38) {
                _that.rotateShape()
            }
            if (key === 39) {
                _that.right()
            }
            if (key === 40) {
                _that.down()
            }
        }    
    },
    mounted () {
        /* this.down()
        setInterval(this.down, 800) */
    },
    methods: {
        // 更新状态
        update (col, row, shape) {
            for (let i = 0; i < 4; i++) {
                let binaryStr = parseInt(shape[i], 16).toString(2)
                if (binaryStr.length < 4) {
                    binaryStr = Array(4 - binaryStr.length).fill(0).join('') + binaryStr
                }
               for (let j = 0; j < binaryStr.length; j++) {
                   if (parseInt(binaryStr[j]) === 1) {
                       this.board[col + i][row + j].status = 1
                   }
               } 
            }
        },
        // 验证边界
        validate (col, row, shape) {
            // console.log(col, row, shape)
            for (let i = 0; i < 4; i++) {
                let binaryStr = parseInt(shape[i], 16).toString(2)
                if (binaryStr.length < 4) {
                    binaryStr = Array(4 - binaryStr.length).fill(0).join('') + binaryStr
                }
                for (let j = 0; j < binaryStr.length; j++) {
                    if (parseInt(binaryStr[j]) === 1) {
                        if (row + j < 0 || row + j > 9) {  
                            console.log(row + j)                      
                            return false
                        }
                        if (col + i < 0 || col + i > 19) {
                            return false
                        } 
                        if (this.board[col + i][row + j].status === 1) {
                            return false
                        }
                    }
                }
            }
        },

        // 组件化 响应式
        draw (col, row, shape, type) {
            for (let i = 0; i < 4; i++) {
                let binaryStr = parseInt(shape[i], 16).toString(2)
                if (binaryStr.length < 4) {
                    binaryStr = Array(4 - binaryStr.length).fill(0).join('') + binaryStr
                }
                for (let j = 0; j < binaryStr.length; j++) {
                    if (parseInt(binaryStr[j]) === 1) {
                        this.board[col + i][row + j].color = type
                        this.board = this.board.slice()
                    }
                }
            }
           // console.log(shape) 
        },
        // 画一个形状
        drawShape (col, row, shape, type) {
            for (let i = 0; i < 4; i++) {
                let binaryStr = parseInt(shape[i], 16).toString(2)
                if (binaryStr.length < 4) {
                    binaryStr = Array(4 - binaryStr.length).fill(0).join('') + binaryStr
                }
                for (let j = 0; j < binaryStr.length; j++) {
                    if (parseInt(binaryStr[j]) === 1) {
                     this.board[col + i][row + j].color = type
                   }  
                }
            }
            this.board = this.board.slice()
        },
        // 下一个图形
        changeShape () {
            this.currentCol = 0
            /* let x = this.currentRow */
            // let currentShape = this.shapes[this.currentRow - 1][this.currentCol]
            this.drawShape(this.currentY, this.currentX, this.shapes[this.currentRow][this.currentCol], 0)
            /* console.log(this.currentRow) */
            this.currentRow = (this.currentRow + 1) % this.shapes.length
            if (this.currentRow === 0) {
                this.currentRow = 1
            }
            this.drawShape(this.currentY, this.currentX, this.shapes[this.currentRow][this.currentCol], this.currentRow)
        },
        // 旋转
        rotateShape () {
            let currentShape = this.shapes[this.currentRow][(this.currentCol + 1) % this.shapes[this.currentRow].length]
            if (this.validate(this.currentY, this.currentX, currentShape) !== false) {
                this.drawShape(this.currentY, this.currentX, this.shapes[this.currentRow][this.currentCol], 0)
                this.currentCol = (this.currentCol + 1) % this.shapes[this.currentRow].length
                this.drawShape(this.currentY, this.currentX, this.shapes[this.currentRow][this.currentCol], this.currentRow)
            } 
             console.log(this.currentCol)
        },   
        
        // 左移
        left () {
            let currentShape = this.shapes[this.currentRow][this.currentCol]
           if (this.validate(this.currentY, this.currentX - 1, currentShape) !== false) {
               this.draw(this.currentY, this.currentX, currentShape, 0)
               this.currentX--
               this.draw(this.currentY, this.currentX, currentShape, this.currentRow)
           }  
        },
        // 右移
        right () {
           let currentShape = this.shapes[this.currentRow][this.currentCol]
           if (this.validate(this.currentY, this.currentX + 1, currentShape) !== false) {
               this.draw(this.currentY, this.currentX, currentShape, 0)
               this.currentX++
               this.draw(this.currentY, this.currentX, currentShape, this.currentRow)
           }           
        },

        // 下移
        down () {
            let currentShape = this.shapes[this.currentRow][this.currentCol]
            if (this.validate(this.currentY + 1, this.currentX, currentShape) !== false) {
                this.draw(this.currentY, this.currentX, currentShape, 0)
                this.currentY++
                this.draw(this.currentY, this.currentX, currentShape, this.currentRow)
            } else {
                this.update(this.currentY, this.currentX, currentShape)
                for (let i = this.board.length - 1; i >= 0; i--) {
                    let flag = true 
                    for (let j = 0; j < this.board[i].length; j++) {
                        if (this.board[i][j].status === 0) {
                            flag = false
                            break
                        }                      
                    }
                    if (flag) {
                        let row = []
                        this.board.splice(i, 1)
                        for (let i = 0; i < 10; i++) {
                            row.push({
                                status: 0,
                                color: 0
                            })
                        }
                        this.board.unshift(row)
                        i++
                    }
                }
                this.currentY = 0
                this.currentX = 0
                this.currentRow = (this.currentRow + 1) % this.shapes.length
                /* console.log(this.currentRow) */
                if (this.currentRow === 0) {
                    this.currentRow = 1
                }
                this.currentCol = 0
            }    
        }
        // 自动向下走
        /* autoDown () {
            let currentShape = this.shapes[this.currentRow][this.currentCol]
            if (this.validate(this.currentY + 1, this.currentX, currentShape) !== false) {
                this.draw(this.currentY, this.currentX, currentShape, 0)
                this.currentY++
                this.draw(this.currentY, this.currentX, currentShape, 1)
            }  else {
                this.update(this.currentY, this.currentX, currentShape)
                for (let i = this.board.length - 1; i >= 0; i--) {
                    let flag = true 
                    for (let j = 0; j < this.board[i].length; j++) {
                        if (this.board[i][j].status === 0) {
                            flag = false
                            break
                        }                      
                    }
                    if (flag) {
                        let row = []
                        this.board.splice(i, 1)
                        for (let i = 0; i < 10; i++) {
                            row.push({
                                status: 0,
                                color: 0
                            })
                        }
                        this.board.unshift(row)
                        i++
                    }
                }
                this.currentY = 0
                this.currentX = 0
                this.currentRow = (this.currentRow + 1) % this.shapes.length
                console.log(this.currentRow)
                if (this.currentRow === 0) {
                    this.currentRow = 1
                }
                this.currentCol = 0  
            }    
        }  */
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

/* 右移 */
#right {
    position: absolute;
    top: 30px;
    left: 10px;
}

/* 下移 */
#down {
    position: absolute;
    top: 30px;
    left: 50px;
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