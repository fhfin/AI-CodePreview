<!-- src/views/DemoPage.vue -->
<script setup lang="ts">
import { ref } from 'vue';
import SecureKeyboardNumber from './components/SecureKeyBoardNumber.vue';
import SecureKeyboardAlphaNumber from './components/SecureKeyboardAlphaNumber.vue';

const amount = ref('');              // 明文，用于展示。真实项目可只存密文
const showNumberKb = ref(false);

const pwdMask = ref('');             // 只展示掩码
const pwdReal = ref('');             // 实际密码（示例用，正式环境建议加密后再存）
const showAlphaKb = ref(false);

const onConfirmAmount = (val: string) => {
  amount.value = val;                // 金额可以直接明文展示
};

const onConfirmPwd = (val: string) => {
  pwdReal.value = val;
  pwdMask.value = '*'.repeat(val.length); // 展示为星号
};
</script>

<template>
  <div class="page">
    <h2>安全键盘 Demo</h2>

    <!-- 数字键盘示例 -->
    <section class="block">
      <h3>数字安全键盘（金额/验证码等）</h3>
      <label class="field-label">金额（只读，点击弹出安全键盘）：</label>
      <input
        class="field-input"
        :value="amount"
        readonly
        inputmode="none"
        autocomplete="off"
        @click="showNumberKb = true"
        placeholder="点击输入金额"
      />

      <p class="tip">当前金额：{{ amount || '未输入' }}</p>

      <SecureKeyboardNumber
        v-model:visible="showNumberKb"
        :maxLength="8"
        title="输入金额"
        @confirm="onConfirmAmount"
      />
    </section>

    <hr />

    <!-- 字母+数字键盘示例 -->
    <section class="block">
      <h3>字母+数字安全键盘（交易密码等）</h3>

      <label class="field-label">密码（展示为 *）：</label>
      <input
        class="field-input"
        :value="pwdMask"
        readonly
        inputmode="none"
        autocomplete="off"
        @click="showAlphaKb = true"
        placeholder="点击输入密码"
      />

      <p class="tip">实际密码（仅为 demo 展示）：{{ pwdReal || '未输入' }}</p>

      <SecureKeyboardAlphaNumber
        v-model:visible="showAlphaKb"
        :maxLength="16"
        title="输入交易密码"
        @confirm="onConfirmPwd"
      />
    </section>
  </div>
</template>

<style scoped>
.page {
  padding: 16px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

.block {
  margin-bottom: 24px;
}

.field-label {
  display: block;
  margin-bottom: 6px;
  font-size: 14px;
}

.field-input {
  width: 100%;
  max-width: 360px;
  padding: 8px 10px;
  font-size: 14px;
  box-sizing: border-box;
  border-radius: 4px;
  border: 1px solid #ccc;
}

.field-input[readonly] {
  background: #f9f9f9;
  cursor: pointer;
}

.tip {
  margin-top: 8px;
  font-size: 13px;
  color: #666;
}
</style>