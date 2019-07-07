<template>
    <div class="box">
        <ul class="puzzle-wrap">
            <li 
                :class="{'puzzle': true, 'puzzle-empty': !puzzle}" 
                v-for="puzzle in puzzles"  :key="puzzle"
                @click="moveFn($index)"
            >{{$index}} -- {{puzzle}} </li>
        </ul>


        <!-- <button class="btn btn-warning btn-block btn-reset" @click="render">重置游戏</button> -->
    </div>
</template>

<script>
export default {
    data () {
        return {
            puzzles: []
        }
    },
    methods: {

        // 重置渲染
        render () {
            let puzzleArr = [];
                // i = 1

            // 生成包含1 ~ 15数字的数组
            for (var i = 1; i < 16; i++) {
                puzzleArr.push(i)
            }

            // 随机打乱数组
            puzzleArr = puzzleArr.sort(() => {
                return Math.random() - 0.5
            });
            
            // 页面显示
            this.puzzles = puzzleArr
            this.puzzles.push('')
        },

        // 点击方块
        moveFn (index) {
            
            // 获取点击位置及其上下左右的值
            let curNum = this.puzzles[index], // 获取当前位置的值
            
                leftNum = this.puzzles[index - 1], // 
                rightNum = this.puzzles[index + 1],
                topNum = this.puzzles[index - 4],
                bottomNum = this.puzzles[index + 4];
            /* console.log('当前点击的值是'+curNum);
            console.log('左值是'+leftNum);
            console.log('右值是'+rightNum);
            console.log('上值是'+topNum);
            console.log('下值是'+bottomNum);
            console.log(index % 4); */


            if (leftNum == '' && index % 4) { // 判断左值
                this.puzzles.$set(index - 1, curNum); // 将当前值设置到左边 
                this.puzzles.$set(index, ''); // 将当前值设为空
            } else if (rightNum == '' && index % 4 !== 3) { // 判断右值
                this.puzzles.$set(index + 1, curNum); // 将当前值设置到右边 
                this.puzzles.$set(index, ''); // 将当前值设为空
            } else if (topNum === '') { // 判断上值
                this.puzzles.$set(index - 4, curNum); // 将当前值设置到上边 
                this.puzzles.$set(index, ''); // 将当前值设为空
            } else if (bottomNum === '') {
                this.puzzles.$set(index + 4, curNum); // 将当前值设置到下边 
                this.puzzles.$set(index, ''); // 将当前值设为空
            }



            this.passFn();
            
            // 和为空的位置交换数值
            /* if (leftNum === '' && index % 4) {
                this.puzzles.$set(index - 1, curNum)
                this.puzzles.$set(index, '')
            } else if (rightNum === '' && 3 !== index % 4) {
                this.puzzles.$set(index + 1, curNum)
                this.puzzles.$set(index, '')
            } else if (topNum === '') {
                this.puzzles.$set(index - 4, curNum)
                this.puzzles.$set(index, '')
            } else if (bottomNum === '') {
                this.puzzles.$set(index + 4, curNum)
                this.puzzles.$set(index, '')
            } */

           
        },

        // 校验是否过关
        passFn () {
            if (this.puzzles[15] === '') {
                const newPuzzles = this.puzzles.slice(0, 15) // 从第一个截取,长度是15 将最后一个过滤掉
                
                
                const isPass = newPuzzles.every((e, i) => e === i + 1)

                if (isPass) {
                    alert ('恭喜，闯关成功！')
                }
            }
        }
    },
    ready () {
        this.render()
    }
}
</script>

<style>
@import url('./assets/css/bootstrap.min.css');

body {
    font-family: Arial, "Microsoft YaHei"; 
}

.box {
    width: 400px;
    margin: 50px auto 0;
}

.puzzle-wrap {
    width: 400px;
    height: 400px;
    margin-bottom: 40px;
    padding: 0;
    background: #ccc;
    list-style: none;
}

.puzzle {
    float: left;
    width: 100px;
    height: 100px;
    font-size: 20px;
    /* background: #f90; */
    background: pink;
    text-align: center;
    line-height: 100px;
    border: 1px solid #ccc;
    box-shadow: 1px 1px 4px;
    text-shadow: 1px 1px 1px #B9B4B4;
    cursor: pointer;
}

.puzzle-empty {
    background: #ccc;
    box-shadow: inset 2px 2px 18px;
}

.btn-reset {
    box-shadow: inset 2px 2px 18px;
}
</style>