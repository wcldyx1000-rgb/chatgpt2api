<template>
  <HoverCard card-class="w-72" focusable>
    <MetaChip :tone="item.health_tone" size="xs" strong>
      {{ item.health_label }}
    </MetaChip>

    <template #content>
      <div class="space-y-2.5 text-xs leading-5">
        <div class="ui-status-title">健康度</div>
        <ul v-if="item.health_reasons.length" class="list-disc space-y-1 pl-4 text-foreground">
          <li v-for="reason in item.health_reasons" :key="reason">{{ reason }}</li>
        </ul>
        <p v-else class="text-muted-foreground">没有发现风险信号</p>
        <dl class="space-y-1 border-t border-border/70 pt-2">
          <div class="flex items-start justify-between gap-4">
            <dt class="text-muted-foreground">近 1 小时使用</dt>
            <dd class="text-right tabular-nums text-foreground">{{ item.recent_uses_1h }} 次</dd>
          </div>
          <div class="flex items-start justify-between gap-4">
            <dt class="text-muted-foreground">近 24 小时使用</dt>
            <dd class="text-right tabular-nums text-foreground">{{ item.recent_uses_24h }} 次</dd>
          </div>
        </dl>
        <p class="text-muted-foreground">近期用量从服务启动开始统计。</p>
      </div>
    </template>
  </HoverCard>
</template>

<script setup lang="ts">
import { HoverCard, MetaChip } from 'nanocat-ui'

import type { Account } from '@/api/accounts'

defineProps<{
  item: Account
}>()
</script>
