<template>
    <main class="flex flex-col items-center gap-3 justify-center min-h-screen md:min-h-[101vh] p-8 bg-gray-200">
        <div class="flex flex-col gap-8 w-full max-w-2xl p-8 bg-white shadow-lg rounded-xl relative">
            <h1 class="text-3xl font-bold text-center">ボゴソート可視化アプリ（β版）</h1>
            <div class="text-center text-gray-700 text-lg font-semibold">
                ソート試行回数: {{ trialCount }}
            </div>
            <canvas ref="canvas" class="w-full h-full border-0 p-3"></canvas>
            <div class="flex justify-center">
                <button class="bg-blue-500 text-white text-lg font-bold py-3 px-8 rounded-lg text-center"
                    @click="startSorting">ボゴソート開始！</button>
            </div>
        </div>
        <div class="w-full max-w-2xl text-right px-8">
            <a class="text-lg font-bold text-gray-500 underline underline-offset-4 cursor-pointer"
                onclick="modal.showModal()">ボゴソートとは？</a>
        </div>
        <dialog id="modal" class="modal">
            <div class="modal-box flex flex-col">
                <h3 class="font-bold text-center text-2xl py-4">ボゴソートとは？</h3>
                <p class="py-4 leading-7">
                    ボゴソートとはソートアルゴリズムの一種であり、配列がソートされているかどうかを確認し、ソートされていない場合はランダムにシャッフルするという手法をソートが完了するまで繰り返します。
                    <br>
                    <br>
                    このアルゴリズムの平均計算時間は O(n×n!) 、最悪計算時間は O(∞) であり、数あるソートアルゴリズムの中でも最悪の性能とされています。
                    <br>
                    <br>
                    しかしその反面、最良計算時間は O(n) というクイックソートを超える最速のソート時間を叩き出すことができるため、理論上は最速のソートアルゴリズムとも言われています...たぶん。
                    <br>

                </p>
            </div>
            <form method="dialog" class="modal-backdrop">
                <button>close</button>
            </form>
        </dialog>
    </main>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';

export default defineComponent({
    name: 'BogoSort',
    setup() {
        const canvas = ref<HTMLCanvasElement | null>(null);
        const arraySize = 5;
        let array: number[] = [];
        let sorting = false;
        let sortInterval: number;
        const trialCount = ref(0); // 追加: 試行回数

        // 配列を初期化
        function initArray() {
            array = [];
            const maxHeight = canvas.value ? canvas.value.height : 400;
            for (let i = 0; i < arraySize; i++) {
                array.push(Math.floor(Math.random() * (maxHeight * 0.9)) + 10);
            }
            trialCount.value = 0; // 追加: 試行回数リセット
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
                        ctx.fillStyle = '#2b7fff';
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
            trialCount.value++; // 追加: 試行回数をインクリメント
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
            startSorting,
            trialCount // 追加: テンプレートで使うため返却
        };
    }
});
</script>