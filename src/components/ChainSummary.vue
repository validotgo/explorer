<script lang="ts" setup>
import { useDashboard } from '@/stores/useDashboard';
import { computed } from 'vue';
import { Icon } from '@iconify/vue';

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
});

const dashboardStore = useDashboard();
const conf = computed(() => dashboardStore.chains[props.name] || {});

const addFavor = (e: Event) => {
  e.stopPropagation();
  e.preventDefault();
  dashboardStore.favoriteMap[props.name] =
    !dashboardStore?.favoriteMap?.[props.name];
  window.localStorage.setItem(
    'favoriteMap',
    JSON.stringify(dashboardStore.favoriteMap)
  );
};
</script>
<template>
  <RouterLink
    :to="`/${name}`"
    class="glass border border-gray-400 dark:border-gray-400 primary hover:bg-gray-100 dark:hover:bg-[#373f59] rounded-md shadow flex items-center px-3 py-3 cursor-pointer"
  >
    <div class="w-8 h-8 rounded-md-full overflow-hidden">
      <img :src="conf.logo" />
    </div>
    <div class="font-semibold ml-4 text-base flex-1 truncate capitalize">
      {{ conf?.prettyName || props.name }}
    </div>
    <div
      @click="addFavor"
      class="pl-4 text-xl"
      :class="{
        'text-warning': dashboardStore?.favoriteMap?.[props.name],
        'text-gray-200 dark:text-gray-500':
          !dashboardStore?.favoriteMap?.[props.name],
      }"
    >
      <Icon icon="mdi-star" />
    </div>
  </RouterLink>
</template>
