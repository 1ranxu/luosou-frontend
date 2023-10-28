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
import { ref, watchEffect } from "vue";
import PostListPage from "@/pages/PostListPage.vue";
import PictureListPage from "@/pages/PictureListPage.vue";
import UsertListPage from "@/pages/UsertListPage.vue";
import MyDivider from "@/pages/MyDivider.vue";
import { useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/myAxios";

const postList = ref([]);
const userList = ref([]);
const pictureList = ref([]);
const initSearchParams = {
  text: "",
  pageSize: 10,
  pageNum: 1,
};
const router = useRouter();
const route = useRoute();

const searchParams = ref(initSearchParams);

/*const loadDataV1 = (params: any) => {
  const postQuery = {
    ...params,
    searchText: searchParams.value.text,
  };
  myAxios.post("/post/list/page/vo", postQuery).then((res: any) => {
    postList.value = res.records;
  });
  const userQuery = {
    ...params,
    userName: searchParams.value.text,
  };
  myAxios.post("/user/list/page/vo", userQuery).then((res: any) => {
    userList.value = res.records;
  });
  const pictureQuery = {
    ...params,
    searchText: searchParams.value.text,
  };
  myAxios.post("/picture/list/page/vo", pictureQuery).then((res: any) => {
    pictureList.value = res.records;
  });
};*/
const loadData = (params: any) => {
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
loadData(initSearchParams);

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
  loadData(searchParams.value);
};

const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });
};
const activeKey = ref(route.params.category);
</script>
