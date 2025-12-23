<!-- src/components/SecureKeyboardAlphaNumber.vue -->
<script setup lang="ts">
import { ref, watch, computed } from 'vue';

const props = defineProps<{
  visible: boolean;
  maxLength?: number;
  title?: string;
}>();

const emits = defineEmits<{
  (e: 'update:visible', value: boolean): void;
  (e: 'confirm', value: string): void;
  (e: 'input', value: string): void;
  (e: 'close'): void;
}>();

const localValue = ref('');
const isUppercase = ref(false);
const activeTab = ref<'number' | 'alpha'>('alpha');

const baseLetters = 'qwertyuiopasdfghjklzxcvbnm'.split('');
const digits = ref<string[]>([]);

function shuffle<T>(arr: T[]): T[] {
  return arr
    .map(v => ({ v, r: Math.random() }))
    .sort((a, b) => a.r - b.r)
    .map(({ v }) => v);
}

const letters = computed(() =>
  (isUppercase.value ? baseLetters.map(l => l.toUpperCase()) : baseLetters)
);

watch(
  () => props.visible,
  (val) => {
    if (val) {
      digits.value = shuffle(['0','1','2','3','4','5','6','7','8','9']);
      localValue.value = '';
      activeTab.value = 'alpha';
      isUppercase.value = false;
    }
  }
);

const onPressChar = (ch: string) => {
  if (props.maxLength && localValue.value.length >= props.maxLength) return;
  localValue.value += ch;
  emits('input', localValue.value);
};

const onDelete = () => {
  if (!localValue.value) return;
  localValue.value = localValue.value.slice(0, -1);
  emits('input', localValue.value);
};

const onClear = () => {
  localValue.value = '';
  emits('input', localValue.value);
};

const onConfirm = () => {
  emits('confirm', localValue.value);
  emits('update:visible', false);
};

const onClose = () => {
  emits('update:visible', false);
  emits('close');
};

const toggleCase = () => {
  isUppercase.value = !isUppercase.value;
};
</script>

<template>
  <teleport to="body">
    <div v-if="visible" class="sk2-mask" @click="onClose">
      <div class="sk2-panel" @click.stop>
        <div class="sk2-header">
          <span>{{ title || '安全字母数字键盘' }}</span>
          <button class="sk2-close-btn" @click="onClose">×</button>
        </div>

        <div class="sk2-display">
          <span v-if="!localValue.length" class="sk2-placeholder">输入中…</span>
          <span v-else class="sk2-dots">
            <span v-for="(ch, idx) in localValue" :key="idx" class="sk2-dot">•</span>
          </span>
        </div>

        <div class="sk2-tabs">
          <button
            type="button"
            :class="['sk2-tab', { active: activeTab === 'alpha' }]"
            @click="activeTab = 'alpha'"
          >
            字母
          </button>
          <button
            type="button"
            :class="['sk2-tab', { active: activeTab === 'number' }]"
            @click="activeTab = 'number'"
          >
            数字
          </button>
          <button
            type="button"
            class="sk2-tab sk2-tab-case"
            @click="toggleCase"
          >
            {{ isUppercase ? '大写' : '小写' }}
          </button>
        </div>

        <!-- 字母区 -->
        <div v-if="activeTab === 'alpha'" class="sk2-alpha">
          <div class="sk2-row">
            <button
              v-for="ch in letters.slice(0,10)"
              :key="ch"
              type="button"
              class="sk2-key"
              @click="onPressChar(ch)"
            >
              {{ ch }}
            </button>
          </div>
          <div class="sk2-row">
            <button
              v-for="ch in letters.slice(10,19)"
              :key="ch"
              type="button"
              class="sk2-key"
              @click="onPressChar(ch)"
            >
              {{ ch }}
            </button>
          </div>
          <div class="sk2-row">
            <button
              v-for="ch in letters.slice(19)"
              :key="ch"
              type="button"
              class="sk2-key"
              @click="onPressChar(ch)"
            >
              {{ ch }}
            </button>
          </div>
          <div class="sk2-row sk2-row-bottom">
            <button type="button" class="sk2-key sk2-key-func" @click="onClear">
              清空
            </button>
            <button type="button" class="sk2-key sk2-key-func" @click="onDelete">
              删除
            </button>
          </div>
        </div>

        <!-- 数字区 -->
        <div v-else class="sk2-number">
          <div class="sk2-grid">
            <button
              v-for="d in digits"
              :key="d"
              type="button"
              class="sk2-key"
              @click="onPressChar(d)"
            >
              {{ d }}
            </button>
          </div>
          <div class="sk2-row sk2-row-bottom">
            <button type="button" class="sk2-key sk2-key-func" @click="onClear">
              清空
            </button>
            <button type="button" class="sk2-key sk2-key-func" @click="onDelete">
              删除
            </button>
          </div>
        </div>

        <button type="button" class="sk2-confirm" @click="onConfirm">
          确定
        </button>
      </div>
    </div>
  </teleport>
</template>

<style scoped>
.sk2-mask {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.35);
  display: flex;
  justify-content: center;
  align-items: flex-end;
  z-index: 9999;
}

.sk2-panel {
  width: 100%;
  max-width: 480px;
  background: #fff;
  border-radius: 12px 12px 0 0;
  padding: 8px 12px 12px;
  box-sizing: border-box;
}

.sk2-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 15px;
  padding: 4px 4px 8px;
  border-bottom: 1px solid #eee;
}

.sk2-close-btn {
  border: none;
  background: transparent;
  font-size: 20px;
  line-height: 1;
  cursor: pointer;
}

.sk2-display {
  min-height: 32px;
  padding: 8px 4px;
  text-align: center;
  font-size: 18px;
}

.sk2-placeholder {
  color: #bbb;
}

.sk2-dots {
  display: inline-flex;
  gap: 4px;
}

.sk2-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #333;
  display: inline-block;
}

.sk2-tabs {
  display: flex;
  margin: 4px 0 8px;
  gap: 6px;
}

.sk2-tab {
  flex: 1;
  padding: 6px 0;
  border-radius: 999px;
  border: 1px solid #ddd;
  background: #f5f5f5;
  font-size: 13px;
  cursor: pointer;
}

.sk2-tab.active {
  background: #1677ff;
  color: #fff;
  border-color: #1677ff;
}

.sk2-tab-case {
  flex: 0 0 72px;
}

.sk2-alpha, .sk2-number {
  padding: 4px 0;
}

.sk2-row {
  display: flex;
  justify-content: center;
  gap: 4px;
  margin-bottom: 4px;
}

.sk2-row-bottom {
  margin-top: 6px;
}

.sk2-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 6px;
}

.sk2-key {
  flex: 1;
  padding: 8px 0;
  min-width: 24px;
  font-size: 16px;
  border-radius: 6px;
  border: 1px solid #ddd;
  background: #fafafa;
  cursor: pointer;
}

.sk2-key-func {
  font-size: 14px;
  background: #f3f3f3;
}

.sk2-confirm {
  width: 100%;
  margin-top: 8px;
  padding: 10px 0;
  border-radius: 6px;
  border: none;
  background: #1677ff;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}
</style>