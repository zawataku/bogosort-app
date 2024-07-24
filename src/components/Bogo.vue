<!-- src/components/BogoSortVisualizer.vue -->
<template>
    <div>
        <canvas ref="canvas" width="800" height="400"></canvas>
        <button @click="startSorting">ソート開始</button>
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';

export default defineComponent({
    name: 'BogoSort',
    setup() {
        const canvas = ref<HTMLCanvasElement | null>(null);
        const arraySize = 10;
        let array: number[] = [];
        let sorting = false;
        let sortInterval: number;

        // 配列を初期化
        function initArray() {
            array = [];
            for (let i = 0; i < arraySize; i++) {
                array.push(Math.floor(Math.random() * 400));
            }
            drawArray();
        }

        // 配列を描画
        function drawArray() {
            if (canvas.value) {
                const ctx = canvas.value.getContext('2d');
                if (ctx) {
                    ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);
                    const barWidth = canvas.value.width / arraySize;
                    for (let i = 0; i < array.length; i++) {
                        ctx.fillStyle = 'blue';
                        ctx.fillRect(i * barWidth, canvas.value.height - array[i], barWidth, array[i]);
                    }
                }
            }
        }

        // ボゴソートのチェック関数
        function isSorted() {
            for (let i = 0; i < array.length - 1; i++) {
                if (array[i] > array[i + 1]) {
                    return false;
                }
            }
            return true;
        }

        // 配列をランダムにシャッフル
        function shuffleArray() {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        // ボゴソートの実行
        function bogoSort() {
            if (!isSorted()) {
                shuffleArray();
                drawArray();
            } else {
                clearInterval(sortInterval);
                sorting = false;
                alert("ソート完了！");
            }
        }

        // 初期化とソートの開始
        function startSorting() {
            if (!sorting) {
                sorting = true;
                initArray();
                sortInterval = setInterval(bogoSort, 100);  // アニメーション速度
            }
        }

        onMounted(() => {
            initArray();
        });

        return {
            canvas,
            startSorting
        };
    }
});
</script>

<style scoped>
canvas {
    border: 1px solid black;
}

button {
    display: block;
    margin: 20px 0;
}
</style>