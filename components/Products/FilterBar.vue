<template>
    <div class="col-3 d-none d-lg-block d-xl-block">
        <div class="card-selector">
            <div class="card-body p-5">
                <div class="search-title">
                    <h4>Cryption:</h4>
                    <h6 v-for="item in info.types" :key="item.name" @click="algorithm(item)" :class="{ 'selected': item === info.selectedAlgorithm }">
                        {{ item.name }} {{ item === info.selectedAlgorithm ? '+' : '' }}
                    </h6>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue';
const info = reactive({
    types: [
        { name: 'AES', value: 'aes' },
        { name: 'ECDSA', value: 'ecdsa' },
        { name: 'SHA256', value: 'sha256' },
        { name: 'SM2', value: 'sm2' },
        { name: 'SM3', value: 'sm3' },
        { name: 'SM4', value: 'sm4' },
    ],
    selectedAlgorithm: { name: 'AES', value: 'aes' },
    colors: [
        { name: 'yellow', value: '#FFC015' },
        { name: 'blue', value: '#829FAA' },
        { name: 'white', value: '#BFB8AE' },
        { name: 'silver', value: '#817A77' }
    ]
});

const emit = defineEmits<{
    (e: 'Alg', alg:{name: string, value:string}): void
}>()

function algorithm(alg:{name: string, value:string}): void {
    emit('Alg', alg);
    info.selectedAlgorithm = alg;
}

// const selectAlgorithm = (algorithm:{name: string, value: string}) => {
//     info.selectedAlgorithm = algorithm;
// };
</script>

<style lang="scss">
.selected {
  font-weight: bold;
}
</style>

<style scoped>
.card-selector {
    color: black;
    font: 20px "Times New Roman", Times, serif; /* 将字体设置为Times Roman */
    height: 35rem;
    background: blanchedalmond !important;
    box-shadow: 0 8px 6px 0 rgba(0, 0, 0, 0.1), 0 26px 70px 0 rgba(0, 0, 0, 0.69);
    border-radius: 10px; /* 设置边框为圆角，这里的10px可以根据需求进行调整 */
}


.search-title {
    margin-bottom: 50px;
    padding: 20px;
}

.search-title h6 {
    cursor: pointer;
}

.circle {
    height: 17px;
    width: 17px;
    border-radius: 50%;
    display: inline-block;
    margin-left: 6px;
    cursor: pointer
}
</style>