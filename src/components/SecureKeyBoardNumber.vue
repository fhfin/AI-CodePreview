<!-- src/components/SecureKeyboardNumber.vue -->
<script setup lang="ts">
import { ref, watch } from 'vue';

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

const localValue = ref<string>('');
const digits = ref<number[]>([]);

function shuffle<T>(arr: T[]): T[] {
  return arr
    .map(v => ({ v, r: Math.random() }))
    .sort((a, b) => a.r - b.r)
    .map(({ v }) => v);
}

watch(
  () => props.visible,
  (val) => {
    if (val) {
      digits.value = shuffle([0,1,2,3,4,5,6,7,8,9]);
      localValue.value = '';
    }
  }
);

const onPressDigit = (d: number) => {
  if (props.maxLength && localValue.value.length >= props.maxLength) return;
  localValue.value += String(d);
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
</script>

<template>
  <teleport to="body">
    <div v-if="visible" class="sk-mask" @click="onClose">
      <div class="sk-panel" @click.stop>
        <div class="sk-header">
          <span>{{ title || '安全数字键盘' }}</span>
          <button class="sk-close-btn" @click="onClose">×</button>
        </div>

        <div class="sk-display">
          <span v-if="!localValue.length" class="sk-placeholder">输入中…</span>
          <span v-else class="sk-dots">
            <span v-for="(ch, index) in localValue" :key="index" class="sk-dot">•</span>
          </span>
        </div>

        <div class="sk-grid">
          <button
            v-for="d in digits"
            :key="d"
            type="button"
            class="sk-key"
            @click="onPressDigit(d)"
          >
            {{ d }}
          </button>

          <!-- 左下：清空 -->
          <button type="button" class="sk-key sk-key-func" @click="onClear">
            清空
          </button>

          <!-- 右下：删除 -->
          <button type="button" class="sk-key sk-key-func" @click="onDelete">
            删除
          </button>
        </div>

        <button type="button" class="sk-confirm" @click="onConfirm">
          确定
        </button>
      </div>
    </div>
  </teleport>
</template>

<style scoped>
.sk-mask {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.35);
  display: flex;
  justify-content: center;
  align-items: flex-end;
  z-index: 9999;
}

.sk-panel {
  width: 100%;
  max-width: 480px;
  background: #fff;
  border-radius: 12px 12px 0 0;
  padding: 8px 12px 12px;
  box-sizing: border-box;
}

.sk-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 15px;
  padding: 4px 4px 8px;
  border-bottom: 1px solid #eee;
}

.sk-close-btn {
  border: none;
  background: transparent;
  font-size: 20px;
  line-height: 1;
  cursor: pointer;
}

.sk-display {
  min-height: 32px;
  padding: 8px 4px;
  text-align: center;
  font-size: 18px;
}

.sk-placeholder {
  color: #bbb;
}

.sk-dots {
  display: inline-flex;
  gap: 4px;
}

.sk-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #333;
  display: inline-block;
}

.sk-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
  padding: 4px 0;
}

.sk-key {
  padding: 10px 0;
  font-size: 18px;
  border-radius: 6px;
  border: 1px solid #ddd;
  background: #fafafa;
  cursor: pointer;
}

.sk-key-func {
  font-size: 14px;
  background: #f3f3f3;
}

.sk-confirm {
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