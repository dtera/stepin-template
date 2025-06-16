<!--suppress ES6UnusedImports, JSUnusedGlobalSymbols -->
<script lang="ts" setup>
import {computed, ref} from 'vue';
import {usePagination} from 'vue-request';
import {all7bColumns, all7bTotal, apiAll7bList} from '@/api/All7bApi.vue';
import VueJsonPretty from 'vue-json-pretty';
import 'vue-json-pretty/lib/styles.css';
import Detail from './Detail.vue';
import {parseJson} from '@/utils/utils';

// 详情弹窗状态
const detailVisible = ref(false);
const currentRecord = ref<any>(null);

// 打开详情弹窗
const showDetail = (record: any) => {
  currentRecord.value = record;
  detailVisible.value = true;
};

// 关闭详情弹窗
const closeDetail = () => {
  detailVisible.value = false;
  currentRecord.value = null;
};

const {
  data,
  run,
  loading,
  current,
  pageSize,
} = usePagination(apiAll7bList, {
  pagination: {
    currentKey: 'pageNum',
    pageSizeKey: 'pageSize',
  },
  defaultParams: [{pageNum: 1, pageSize: 10}]
});

const pagination = computed(() => ({
  total: all7bTotal.value,
  current: current.value,
  pageSize: pageSize.value,
  showSizeChanger: true,
  showQuickJumper: true,
  showTotal: (total: number) => `共 ${total} 条`
}));

const handleTableChange = (
    pag: { pageSize: number; current: number },
    filters: any,
    sorter: any,
) => {
  run({
    pageSize: pag.pageSize,
    pageNum: pag.current,
    sortField: sorter.field,
    sortOrder: sorter.order,
    ...filters,
  });
};

const dataSource = computed(() => data.value || []);

</script>

<template>
  <a-table
      :columns="all7bColumns"
      :data-source="dataSource"
      :pagination="pagination"
      :loading="loading"
      @change="handleTableChange"
  >
    <template #bodyCell="{ column, text, record }">
      <template v-if="column.key === 'action'">
        <a-space size="middle">
          <a @click="showDetail(record)">详情</a>
        </a-space>
      </template>
      <template v-else-if="column.key === 'prod_img_url'">
        <a-image
            :width="60"
            :src="text.split(',')[0]"
        />
      </template>
      <template v-else-if="column.key === 'prod_detail'">
        <div>{{ parseJson(text) }}</div>
      </template>
      <template v-else-if="column.key === 'prod_llm_pred_names'">
        <vue-json-pretty
            v-if="typeof parseJson(text, true) === 'object'"
            :data="parseJson(text, true)"
            :deep="3"
        />
        <span v-else>{{ parseJson(text, true) }}</span>
      </template>

    </template>
  </a-table>

  <!-- 详情弹窗组件 -->
  <Detail
      v-model:visible="detailVisible"
      :record="currentRecord"
      @close="closeDetail"
  />
</template>
