<template>
  <section
    v-if="health && health.warnings.length"
    class="rounded-md border px-3 py-2.5 text-sm"
    :class="health.level === 'danger'
      ? 'border-rose-300/70 bg-rose-50 text-rose-800 dark:border-rose-700/70 dark:bg-rose-950/30 dark:text-rose-200'
      : 'border-amber-300/70 bg-amber-50 text-amber-800 dark:border-amber-700/70 dark:bg-amber-950/30 dark:text-amber-200'"
    role="status"
    aria-label="号池预警"
  >
    <div class="flex flex-wrap items-start justify-between gap-x-4 gap-y-2">
      <div class="flex min-w-0 items-start gap-2">
        <Icon icon="lucide:triangle-alert" class="mt-0.5 h-4 w-4 shrink-0" />
        <ul class="min-w-0 space-y-0.5">
          <li v-for="warning in health.warnings" :key="warning.code">{{ warning.message }}</li>
        </ul>
      </div>
      <slot name="action" />
    </div>
    <p class="mt-1.5 pl-6 text-xs opacity-80">
      近 1 小时请求 {{ health.metrics.demand_1h }} 次，号池约可承载 {{ health.metrics.capacity_1h }} 次；可用账号
      {{ health.metrics.ready_accounts }} / {{ health.metrics.total_accounts }}<template v-if="observedMinutes !== null">；近期数据只覆盖服务启动后的 {{ observedMinutes }} 分钟</template>
    </p>
  </section>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { Icon } from '@iconify/vue'

import type { AccountPoolHealth } from '@/api/accounts'

const props = defineProps<{
  health: AccountPoolHealth | null | undefined
}>()

const observedMinutes = computed(() => {
  const seconds = Number(props.health?.metrics.observed_seconds ?? 3600)
  return seconds < 3600 ? Math.max(1, Math.floor(seconds / 60)) : null
})
</script>
