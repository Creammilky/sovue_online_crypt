<template>
  
  <div class="col-11 col-md-12 col-lg-8 mx-auto" style="margin-left:25px !important">
    <div class="input-group">
      <input type="text" v-model="Enc" placeholder="Input plaintext..." class="input-text" />
      <br>
      <input type="text" v-model="Dec" placeholder="Input cyphertext..." class="input-text" />
      <div class="button-group">
        <button @click="encrypt">Encryption</button>
        <button @click="decrypt">Decryption</button>
        <button @click="reverse">Reverse</button>
      </div>
    </div>
  </div>
  <ProductsFilterBar @Alg="handleAlgorithm" />
</template>

<script setup lang="ts">
import { Product } from '../types';
import { ref } from 'vue';
import { encryptText, decryptText, } from '@/store/index'; // 假设有后端 API 请求函数
import { defineProps } from 'vue';
const props = defineProps<{
  cards: Product[]
}>();

const Enc = ref('');
const Dec = ref('');

const selectedAlgorithm = ref({ name: 'AES', value: 'aes' }); // 新增一个变量来存储子组件发射的选中算法

const handleAlgorithm = (alg: { name: string, value:string}) => {
  selectedAlgorithm.value = alg; // 接收子组件发射的算法信息
};
const encrypt = async () => {
  try {
    const encryptedResult = await encryptText(Enc.value, selectedAlgorithm.value); // 调用后端加密接口
    Dec.value = encryptedResult;
    console.log("Success enctypted: \"", Enc.value, "\"to\"", encryptedResult, "\"", "by", selectedAlgorithm.value);
  } catch (error) {
    console.error('Encryption failed:', error);
  }
};

const decrypt = async () => {
  try {
    const decryptedResult = await decryptText(Dec.value, selectedAlgorithm.value); // 调用后端解密接口
    Enc.value = decryptedResult;
    console.log("Success dnctypted: \"", Dec.value, "\"to\"", decryptedResult, "\"", "by", selectedAlgorithm.value);
  } catch (error) {
    console.error('Decryption failed:', error);
  }
};

const reverse = () => {
  Dec.value = Enc.value.split('').reverse().join(''); // 简单的文本反转
};
</script>


<style scoped>
.text-input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.input-group {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
}

.input-text {
  width: 100%;
  height: 200px;
  padding: 5px;
}

.button-group {
  display: flex;
  justify-content: center;
  /* 水平居中 */
  align-items: center;
}

button {
  padding: 15px 30px;
  margin: 30px;
}

.button-group {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px; /* 添加一些底部边距，以便与其他内容分隔开 */
}

.button-group button {
  border: none; /* 移除按钮的默认边框 */
  border-radius: 5px; /* 设置按钮的圆角 */
  background-color: darkgray; /* 设置按钮的背景颜色 */
  color: white; /* 设置按钮的文字颜色为白色 */
  cursor: pointer; /* 将鼠标光标设置为指针样式 */
  transition: background-color 0.3s ease; /* 添加过渡效果 */
}

.button-group button:hover {
  background-color: grey; /* 悬停时改变按钮的背景颜色 */
}

</style>
