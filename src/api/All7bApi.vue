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
    dataIndex: 'prod_title',
    key: 'prod_title',
    width: "15%",
  },
  {
    title: '商品图片',
    dataIndex: 'prod_img_url',
    key: 'prod_img_url',
    width: "10%",
  },
  {
    title: '品类',
    key: 'prod_type',
    dataIndex: 'prod_type',
    width: "10%",
  },
  {
    title: '商详',
    key: 'prod_detail',
    dataIndex: 'prod_detail',
    width: "20%",
    customCell: () => ({style: {wordBreak: 'break-all'}}),
  },
  {
    title: '大模型推理商品名',
    key: 'prod_llm_pred_names',
    dataIndex: 'prod_llm_pred_names',
    width: "30%",
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
    prod_title: string;
    prod_img_url: string;
    prod_type: string;
    prod_detail: string;
    prod_llm_pred_names: string;
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
