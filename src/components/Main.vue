<template>
    <div class="main-container">
        <div class="container">
            <span v-for="i in range" :key="i">
                <span v-for="j in range" :key="j">
                    <div id="box">
                        <button :id="i + '-' + j" class="button" v-on:click="change(j, i)">

                        </button>
                    </div>
                </span>
            </span>
        </div>
    </div>
    <div id="button-container">
        <div>
            <button id="bottom-button" v-on:click="calculateDikstra_v2()">
                Calculate
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Main',
    data() {
        return {
            range: 20,
            selected: Array.from({ length: 20 }, () => Array(20).fill(0)),
            vis: Array.from({ length: 20 }, () => Array(20).fill(0)),
            dp: Array.from({ length: 20 }, () => Array(20).fill(10000000)),
            minPath: 100000,
            minPathAdj: [],
            d1: [-1, -1, 0, 0, -1, 1, 1, 1],
            d2: [0, -1, -1, 1, 1, 1, 0, -1],
            d: ['U', 'A', 'L', 'R', 'B', 'C', 'D', 'E'],
            ans: 10000000,
            route: "",
        };
    },
    mounted() {
        for (let i = 0; i < this.range; i++) {
            for (let j = 0; j < this.range; j++) {
                this.selected[i][j] = 0;
            }
        }
    },
    methods: {
        change(i, j) {
            console.log(i, j);
            if ((i == 1 && j == 1) || (i == this.range && j == this.range)) {
                alert("You can't select this box");
                return;
            }
            this.selected[i - 1][j - 1] = 1;
            this.vis[i - 1][j - 1] = 1;
            const elementId = `${j}-${i}`;
            const element = document.getElementById(elementId);
            element.style.backgroundColor = '#2980b9';
        },
        calculateDikstra_v2() {
            const start = Date.now();
            console.log("Start :", start);
            this.dfs(0, 0, 0, "");
            const end = Date.now();
            console.log("End :", end);
            console.log(`Execution time: ${end - start} ms`);
            console.log(this.route);
            this.showFinalPath();
        },
        async showFinalPath() {
            let i = 0;
            let j = 0;
            const elementId = `${j + 1}-${i + 1}`;
            console.log(elementId);
            const element = document.getElementById(elementId);
            element.style.backgroundColor = '#008000';
            for (let k = 0; k < this.route.length; k++) {
                if (this.route[k] == 'U') {
                    i = i - 1;
                }
                if (this.route[k] == 'D') {
                    i = i + 1
                }
                if (this.route[k] == 'R') {
                    j = j + 1;
                }
                if (this.route[k] == 'L') {
                    j = j - 1;
                }
                if (this.route[k] == 'A') {
                    j = j - 1;
                    i = i - 1;
                }
                if (this.route[k] == 'B') {
                    j = j + 1;
                    i = i - 1;
                }
                if (this.route[k] == 'C') {
                    i = i + 1;
                    j = j + 1;
                }
                if (this.route[k] == 'E') {
                    j = j - 1;
                    i = i + 1;
                }
                const elementId = `${j + 1}-${i + 1}`;
                console.log(elementId);
                const element = document.getElementById(elementId);
                element.style.backgroundColor = '#008000';

                await new Promise(resolve => setTimeout(resolve, 500));
            }
        },
        ok(i, j) {
            return i >= 0 && i < this.range && j >= 0 && j < this.range && this.vis[i][j] === 0;
        },
        dfs(i, j, c, s) {
            if (i == this.range - 1 && j == this.range - 1) {
                if (c < this.ans) {
                    this.ans = c;
                    this.route = s;
                }
                console.log(s);
                return;
            }
            this.vis[i][j] = 1;
            console.log(i, j);
            for (let k = 0; k < 8; k++) {
                let x = i + this.d1[k];
                let y = j + this.d2[k];
                if (this.ok(x, y) && c + 1 < this.ans && c+1 < this.dp[x][y]) {
                    this.vis[x][y] = 1;
                    this.dp[x][y]=c+1;
                    this.dfs(x, y, c + 1, s + this.d[k]);
                    this.vis[x][y] = 0;
                }
            }
        }
    }
}
</script>

<style scoped>
.main-container {
    display: flex;
    justify-content: center;
    align-content: center;
    height: 100vh;

}

.container {
    display: flex;
    align-items: center;
    flex-direction: row;
    flex-wrap: wrap;
}

.button {
    width: 30px;
    height: 30px;
}

#button-container {
    margin-top: -150px;
    display: flex;
    justify-content: center;
}
</style>