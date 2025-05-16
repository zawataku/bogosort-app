<template>
    <main class="flex flex-col items-center gap-3 justify-center min-h-[101vh] p-8 bg-gray-200">
        <div class="flex flex-col gap-8 w-full max-w-2xl p-8 bg-white shadow-lg rounded-xl relative">
            <h1 class="text-3xl font-bold text-center">ボゴソート可視化アプリ</h1>
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
            <div class="modal-box">
                <h3 class="text-lg font-bold">ボゴソートとは？</h3>
                <p class="py-4">
                    ボゴソートは、非常に非効率なソートアルゴリズムで、配列がソートされているかどうかを確認し、ソートされていない場合はランダムにシャッフルするという手法を繰り返します。<br>
                    このアルゴリズムは、平均的な時間計算量が
                    $$
                    \begin{eqnarray}
                    \sum^n_{k=1}a_k = S_n = a_1 + a_2 + ....+a_n
                    \end{eqnarray}
                    $$
                    であり、実用的ではありませんが、教育目的やジョークとして使われることがあります。
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

        // 配列を初期化
        function initArray() {
            array = [];
            // Canvasの高さを基準に最大値を決定
            const maxHeight = canvas.value ? canvas.value.height : 400;
            for (let i = 0; i < arraySize; i++) {
                // バーの高さがCanvas内に収まるように調整
                array.push(Math.floor(Math.random() * (maxHeight * 0.9)) + 10);
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