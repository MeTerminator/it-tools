<script setup lang="ts">
import { IconDragDrop, IconFileDescription, IconHeart } from '@tabler/icons-vue';
import { useHead } from '@vueuse/head';
import { computed } from 'vue';
import Draggable from 'vuedraggable';
import ColoredCard from '../components/ColoredCard.vue';
import ToolCard from '../components/ToolCard.vue';
import { useToolStore } from '@/tools/tools.store';
import { config } from '@/config';

const toolStore = useToolStore();

useHead({ title: 'IT Tools - Handy online tools for developers' });
const { t } = useI18n();

const favoriteTools = computed(() => toolStore.favoriteTools);

// Update favorite tools order when drag is finished
function onUpdateFavoriteTools() {
  toolStore.updateFavoriteTools(favoriteTools.value); // Update the store with the new order
}
</script>

<template>
  <div class="pt-50px">
    <div class="grid-wrapper">
      <div class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4">
        <ColoredCard v-if="config.showBanner" title="MeT-Tools" :icon="IconHeart">
          MeT-Tools | 
          <a
            href="https://www.met6.top:444/"
            rel="noopener"
            target="_blank"
            aria-label="MeT-Home"
          >MeT-Home</a>
        </ColoredCard>

        <a href="/ps/" target="_blank" rel="noopener" class="text-current decoration-none">
          <c-card v-if="config.showSponsorBanner" class="cursor-pointer !border-2px !hover:border-primary">
            <div class="flex items-center justify-between">
              <n-icon :component="IconFileDescription" class="text-neutral-400 dark:text-neutral-600" size="40" />
              <div class="rounded-full bg-#eeeeee px-10px py-2px text-xs text-black dark:bg-#333333 dark:text-white">
                工具
              </div>
            </div>

            <div class="my-5px flex items-baseline gap-4 text-balance text-lg text-black dark:text-white">
              在线 PS
            </div>
            <div class="text-neutral-500 dark:text-neutral-400">
              在线 Photoshop 工具. 在浏览器上快速处理图片.
            </div>
          </c-card>
        </a>

        <a href="/mcrgb/" target="_blank" rel="noopener" class="text-current decoration-none">
          <c-card v-if="config.showSponsorBanner" class="cursor-pointer !border-2px !hover:border-primary">
            <div class="flex items-center justify-between">
              <n-icon :component="IconFileDescription" class="text-neutral-400 dark:text-neutral-600" size="40" />
              <div class="rounded-full bg-#eeeeee px-10px py-2px text-xs text-black dark:bg-#333333 dark:text-white">
                工具
              </div>
            </div>

            <div class="my-5px flex items-baseline gap-4 text-balance text-lg text-black dark:text-white">
              Minecraft RGB 文字工具
            </div>
            <div class="text-neutral-500 dark:text-neutral-400">
              改自 RGBirdflop 工具, 在浏览器里生成 Minecraft 渐变文字, 支持多种格式.
            </div>
          </c-card>
        </a>
      </div>

      <transition name="height">
        <div v-if="toolStore.favoriteTools.length > 0">
          <h3 class="mb-5px mt-25px text-neutral-400 font-500">
            {{ $t('home.categories.favoriteTools') }}
            <c-tooltip :tooltip="$t('home.categories.favoritesDndToolTip')">
              <n-icon :component="IconDragDrop" size="18" />
            </c-tooltip>
          </h3>
          <Draggable
            :list="favoriteTools"
            class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4"
            ghost-class="ghost-favorites-draggable"
            item-key="name"
            @end="onUpdateFavoriteTools"
          >
            <template #item="{ element: tool }">
              <ToolCard :tool="tool" />
            </template>
          </Draggable>
        </div>
      </transition>

      <div v-if="toolStore.newTools.length > 0">
        <h3 class="mb-5px mt-25px text-neutral-400 font-500">
          {{ t('home.categories.newestTools') }}
        </h3>
        <div class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4">
          <ToolCard v-for="tool in toolStore.newTools" :key="tool.name" :tool="tool" />
        </div>
      </div>

      <h3 class="mb-5px mt-25px text-neutral-400 font-500">
        {{ $t('home.categories.allTools') }}
      </h3>
      <div class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4">
        <ToolCard v-for="tool in toolStore.tools" :key="tool.name" :tool="tool" />
      </div>
    </div>
  </div>
</template>

<style scoped lang="less">
.height-enter-active,
.height-leave-active {
  transition: all 0.5s ease-in-out;
  overflow: hidden;
  max-height: 500px;
}

.height-enter-from,
.height-leave-to {
  max-height: 42px;
  overflow: hidden;
  opacity: 0;
  margin-bottom: 0;
}

.ghost-favorites-draggable {
  opacity: 0.4;
  background-color: #ccc;
  border: 2px dashed #666;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  transform: scale(1.1);
  animation: ghost-favorites-draggable-animation 0.2s ease-out;
}

@keyframes ghost-favorites-draggable-animation {
  0% {
    opacity: 0;
    transform: scale(0.9);
  }
  100% {
    opacity: 0.4;
    transform: scale(1.0);
  }
}
</style>
