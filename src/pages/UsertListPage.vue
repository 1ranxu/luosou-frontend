<script setup lang="ts">
import { defineProps, withDefaults } from "vue";
import luoying from "@/assets/luoying.png";

interface Props {
  userlist: any[];
}

const props = withDefaults(defineProps<Props>(), {
  userlist: () => [],
});
</script>

<template>
  <div>
    <a-list item-layout="horizontal" :data-source="props.userlist">
      <template #renderItem="{ item }">
        <a-list-item>
          <a-card hoverable style="width: 240px">
            <template #cover>
              <img alt="example" :src="luoying" />
            </template>
            <a-card-meta :title="item.userName">
              <template #description>{{ item.userProfile }}</template>
            </a-card-meta>
          </a-card>
        </a-list-item>
      </template>
    </a-list>
  </div>
</template>

<style scoped></style>
