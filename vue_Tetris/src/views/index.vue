<template>
  <div>
    <div v-for="i in 20" :key="i" class="row">
      <square v-for="j in 10" :key="j" :green="board[i - 1][j - 1]"></square>
    </div>
    <button @click="left">左移</button>
  </div>
</template>
<script>
import square from '../components/square.vue'
export default {
    components: {
        square
    },
    data() {
        return {
            board: Array(20),
            current: 0,
            shapes : [
                    /* O I L Z S J T */
                    // shapeO
                    ["CC00", "CC00", "CC00", "CC00"],
                    //shapeI
                    ["8888", "F000", "8888", "F000"],
                    //spapeL
                    ["88C0", "2E00", "C440", "E800"],
                    //shapeZ
                    ["C600", "4C80", "C600", "4C80"],
                    //shapeS
                    ["6C00", "8C40", "6C00", "8C40"],
                    //shapeT
                    ["E400", "8C80", "4E00", "4C40"],
                    //shapeJ
                    ["44C0", "E200", "C880", "8E00"]
                ]

        }
    },
    // 生命周期 钩子
    created() {
        for(let i = 0; i < 20; i++) {
            let row = [];
            for(let j = 0; j < 10; j++) {
                row.push(0)
            }
            this.board[i] = row;
        }
        this.board[0][0] = 1;
        this.board[1][0] = 1;
        this.board[1][1] = 1;
        this.board[1][2] = 1;
    },
    methods: {
        left() {
            this.draw(0);
            this.current++;
            if(this.current > 7) {
                this.current = 0;
            }
            this.draw(1);
        },
        // 组件化 响应式
        draw(type) {
            this.board[0][this.current] = type;
            this.board[1][this.current] = type;
            this.board[1][this.current + 1] = type;
            this.board[1][this.current + 2] = type;
            this.board = this.board.slice();
        }
    }
}
</script>
<style>
.row {
  height: 20px;
}
</style>