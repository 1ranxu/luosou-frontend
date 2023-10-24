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
        <PictureListPage />
      </a-tab-pane>
      <a-tab-pane key="user" tab="用户">
        <UsertListPage :userlist="userList" />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostListPage from "@/pages/PostListPage.vue";
import PictureListPage from "@/pages/PictureListPage.vue";
import UsertListPage from "@/pages/UsertListPage.vue";
import MyDivider from "@/pages/MyDivider.vue";
import { useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/myAxios";

myAxios.post("/post/list/page/vo", {}).then((res: any) => {
  postList.value = res.records;
});

myAxios.post("/user/list/page/vo", {}).then((res: any) => {
  userList.value = res.records;
});

const router = useRouter();

const route = useRoute();

const initSearchParams = {
  text: "",
  pageSize: 10,
  pageNum: 1,
};
const searchParams = ref(initSearchParams);

const postList = ref([]);
const userList = ref([]);

watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.query.text as string,
  } as any;
});

const onSearch = (value: string) => {
  router.push({
    query: searchParams.value,
  });
};

const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });
};
const activeKey = ref(route.params.category);
</script>
