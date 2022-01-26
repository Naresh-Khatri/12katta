<template>
    <div class="cell" :class="kattaArray.includes(cellNum) ? 'bg-green' : 'bg-blue-10'">
        <!-- {{ kattaArray.includes(cellNum) ? 'katta' : '' }} -->
        <div class="bg-green" v-if="cellNum == player1Path[pos1]">(1)</div>
        <div class="bg-red" v-if="cellNum == player2Path[pos2]">(2)</div>
        <!-- <div class="bg-primary text-white" v-if="kattaArray.includes(cellNum)">x</div> -->
        <div v-if="!kattaArray.includes(cellNum)" style="">{{ cellNum }}</div>
        <div v-else>
            <q-icon size="lg" name="close" />
        </div>
    </div>
</template>
<script setup>
import { ref, onMounted, defineProps } from 'vue'
onMounted(() => {
    // console.log('hi', props)
})
const props = defineProps(['cellNum', 'pos1', 'pos2']);
const kattaArray = ref([4, 9, 13, 22, 25, 28, 37, 41, 46])
const player1Path = ref([
    46, 47, 48, 49, 42, 35, 28, 21, 14, 7, 6, 5, 4, 3, 2, 1, 8, 15, 22, 29,
    36, 43, 44, 45, 37, 30, 23, 16, 9, 10, 11, 12, 13, 20, 27, 34, 41, 40,
    39, 38, 31, 24, 17, 18, 19, 26, 33, 32, 25,
]);
const player2Path = ref([
    4, 3, 2, 1, 8, 15, 22, 29, 36, 43, 44, 45, 46, 47, 48, 49, 42, 35, 28,
    21, 14, 7, 6, 5, 13, 20, 27, 34, 41, 40, 39, 38, 37, 30, 23, 16, 9, 10,
    11, 12, 19, 26, 33, 32, 31, 24, 17, 18, 25,
])
</script>
<style scoped>
.cell {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 70px;
    width: 70px;
    text-align: center;
    padding: 10px 20px;
    border: 1px solid gray;
    /* border-radius: 15px; */
    color: white;
}
</style>