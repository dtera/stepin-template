<!--suppress JSUnusedGlobalSymbols -->
<script lang="ts">
import axios from 'axios';
import {ref} from "vue";

export const all7bColumns = [
  {
    title: '商品ID',
    dataIndex: 'id',
    key: 'id',
    width: "10%",
  },
  {
    title: '商品title',
    dataIndex: 'prodTitle',
    key: 'prodTitle',
    width: "15%",
  },
  {
    title: '商品图片',
    dataIndex: 'prodImgUrl',
    key: 'prodImgUrl',
    width: "10%",
  },
  {
    title: '品类',
    key: 'prodType',
    dataIndex: 'prodType',
    width: "6%",
  },
  {
    title: '商详',
    key: 'prodDetail',
    dataIndex: 'prodDetail',
    width: "20%",
    customCell: () => ({style: {wordBreak: 'break-all'}}),
  },
  {
    title: '小店标准商品名',
    key: 'normalName',
    dataIndex: 'normalName',
    width: "9%",
  },
  {
    title: '文章推理L3商品名',
    key: 'bizL3Name',
    dataIndex: 'bizL3Name',
    width: "9%",
  },
  {
    title: '文章推理品牌名',
    key: 'brandBiz',
    dataIndex: 'brandBiz',
    width: "8%",
  },
  {
    title: '商品最低价',
    key: 'skuMinPrice',
    dataIndex: 'skuMinPrice',
    width: "8%",
  },
  {
    title: '操作',
    key: 'action',
    width: "5%",
  },
];

type APIParams = {
  pageNum?: number;
  pageSize?: number;
};

type APIResult = {
  records: {
    id: string;
    key: string;
    prodTitle: string;
    prodImgUrl: string;
    prodType: string;
    prodDetail: string;
    prodLlmPredNames: string;
  }[];
  total: number;
  size: number;
  current: number;
  pages: number;
};

export const all7bTotal = ref(0);

export const apiAll7bList = async (params: APIParams) => {
  let res = await axios.get<APIResult>('/api/all7b/list', {
    params,
    headers: {"Content-Type": "application/json"}
  });
  all7bTotal.value = res.data.total;
  return res.data.records;
};

</script>
