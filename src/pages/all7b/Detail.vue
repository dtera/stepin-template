<!-- Detail.vue -->
<!--suppress ALL -->
<template>
  <!-- 详情弹窗 -->
  <a-modal
      v-model:visible="props.visible"
      title="商品详情"
      width="800px"
      :footer="null"
      @cancel="close"
      centered
  >
    <div v-if="record" class="detail-container">
      <a-row class="detail-row">
        <a-col :span="6" class="detail-label">商品ID：</a-col>
        <a-col :span="18" class="detail-value">{{ record.id }}</a-col>
      </a-row>

      <a-row class="detail-row">
        <a-col :span="6" class="detail-label">商品title：</a-col>
        <a-col :span="18" class="detail-value">{{ record.prodTitle }}</a-col>
      </a-row>

      <a-row class="detail-row">
        <a-col :span="6" class="detail-label">商品图片：</a-col>
        <a-col :span="18" class="detail-value">
          <a-image
              :width="100"
              :src="record.prodImgUrl?.split(',')[0]"
          />
        </a-col>
      </a-row>

      <a-row class="detail-row">
        <a-col :span="6" class="detail-label">品类：</a-col>
        <a-col :span="18" class="detail-value">{{ record.prodType }}</a-col>
      </a-row>

      <a-row class="detail-row">
        <a-col :span="6" class="detail-label">商详：</a-col>
        <a-col :span="18" class="detail-value">
          <div>{{ parseJson(record.prodDetail) }}</div>
        </a-col>
      </a-row>

      <a-row class="detail-row">
        <a-col :span="6" class="detail-label">大模型推理商品名：</a-col>
        <a-col :span="18" class="detail-value">
          <vue-json-pretty
              v-if="typeof parseJson(record.prodLlmPredNames, true) === 'object'"
              :data="parseJson(record.prodLlmPredNames, true)"
              :deep="3"
          />
          <span v-else>{{ parseJson(record.prodLlmPredNames, true) }}</span>
        </a-col>
      </a-row>
    </div>

    <template #footer>
      <a-button type="primary" @click="close">关闭</a-button>
    </template>
  </a-modal>
</template>

<script lang="ts" setup>
import {computed} from 'vue';
import VueJsonPretty from 'vue-json-pretty';
import 'vue-json-pretty/lib/styles.css';
import {parseJson} from '@/utils/utils';

const props = defineProps({
  visible: Boolean,
  record: Object
});

const emit = defineEmits(['update:visible', 'close']);

// 计算属性获取当前记录
const record = computed(() => props.record || {});

// 关闭弹窗
const close = () => {
  emit('update:visible', false);
  emit('close');
};

</script>

<style scoped>
.detail-container {
  padding: 10px;
}

.detail-row {
  margin-bottom: 16px;
  border-bottom: 1px dashed #f0f0f0;
  padding-bottom: 12px;
}

.detail-label {
  font-weight: bold;
  color: #666;
}

.detail-value {
  word-break: break-word;
}

/* 为JSON格式化添加滚动条 */
:deep(.vjs-tree) {
  max-height: 300px;
  overflow: auto;
  border: 1px solid #e8e8e8;
  padding: 10px;
  border-radius: 4px;
  background-color: #fafafa;
}
</style>