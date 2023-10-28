<script setup lang="ts">
import luoying from "@/assets/luoying.png";
import { defineProps, withDefaults } from "vue";

interface Props {
  picturelist: any[];
}

const props = withDefaults(defineProps<Props>(), {
  picturelist: () => [],
});
</script>

<template>
  <div>
    <a-list
      :grid="{ gutter: 16, xs: 1, sm: 2, md: 4, lg: 4, xl: 6, xxl: 3 }"
      item-layout="horizontal"
      :data-source="props.picturelist"
    >
      <template #renderItem="{ item }">
        <a-list-item>
          <a-card hoverable>
            <template #cover>
              <img alt="example" :src="item.url" />
            </template>
            <a-card-meta :title="item.title"> </a-card-meta>
          </a-card>
        </a-list-item>
      </template>
    </a-list>
  </div>
</template>

<style scoped></style>
