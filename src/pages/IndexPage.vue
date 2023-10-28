<template>
  <div class="index">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search loading with enterButton"
      size="large"
      enter-button="Search"
      @search="onSearch"
    />
    <MyDivider />
    <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
      <a-tab-pane key="post" tab="文章">
        <PostListPage :postlist="postList" />
      </a-tab-pane>
      <a-tab-pane key="picture" tab="图片">
        <PictureListPage :picturelist="pictureList" />
      </a-tab-pane>
      <a-tab-pane key="user" tab="用户">
        <UsertListPage :userlist="userList" />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, watchEffect } from "vue";
import PostListPage from "@/pages/PostListPage.vue";
import PictureListPage from "@/pages/PictureListPage.vue";
import UsertListPage from "@/pages/UsertListPage.vue";
import MyDivider from "@/pages/MyDivider.vue";
import { useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/myAxios";
import { message } from "ant-design-vue";

const postList = ref([]);
const userList = ref([]);
const pictureList = ref([]);
const router = useRouter();
const route = useRoute();
const activeKey = ref(route.params.category);
const initSearchParams = {
  type: activeKey.value,
  text: "",
  pageSize: 10,
  pageNum: 1,
};
const searchParams = ref(initSearchParams);
//加载聚合数据
const loadAllData = (params: any) => {
  const query = {
    ...params,
    searchText: searchParams.value.text,
  };
  myAxios.post("/search/all", query).then((res: any) => {
    postList.value = res.postList;
    userList.value = res.userList;
    pictureList.value = res.pictureList;
  });
};
//加载单类数据
const loadData = (params: any) => {
  const { type } = searchParams.value;
  if (!type) {
    message.error("类别为空");
    return;
  }
  const query = {
    ...params,
    type: type,
    searchText: searchParams.value.text,
  };
  myAxios.post("/search/all", query).then((res: any) => {
    if (type === "post") {
      postList.value = res.dataList;
    }
    if (type === "user") {
      userList.value = res.dataList;
    }
    if (type === "picture") {
      pictureList.value = res.dataList;
    }
  });
};
onMounted(() => {
  loadData(searchParams.value);
});
watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.query.text as string,
    type: route.query.type as string,
  } as any;
});

const onSearch = () => {
  router.push({
    query: searchParams.value,
  });
  loadData(searchParams.value);
};

const onTabChange = (key: string) => {
  searchParams.value.type = key;
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });

  loadData(searchParams.value);
};
</script>
