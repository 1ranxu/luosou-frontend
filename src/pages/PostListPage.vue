<script setup lang="ts">
import { defineProps, withDefaults } from "vue";
import luoying from "@/assets/luoying.png";

interface Props {
  postlist: any[];
}

const props = withDefaults(defineProps<Props>(), {
  postlist: () => [],
});
</script>

<template>
  <div>
    <a-list item-layout="horizontal" :data-source="props.postlist">
      <template #renderItem="{ item }">
        <a-list-item>
          <a-list-item-meta :description="item.content">
            <template #title>
              <a>{{ item.title }}</a>
            </template>
            <template #avatar>
              <a-avatar :src="luoying" />
            </template>
          </a-list-item-meta>
        </a-list-item>
      </template>
    </a-list>
  </div>
</template>

<style scoped></style>
