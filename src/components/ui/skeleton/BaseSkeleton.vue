<script setup lang="ts">
import { cn } from '@/lib/utils';

interface SkeletonProps {
  class?: string;
}

const props = defineProps<SkeletonProps>();
</script>

<template>
  <div :class="cn('animate-pulse  bg-muted', props.class)" />
</template>
