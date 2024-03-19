<script setup lang="ts">
import { cn } from '@/lib/utils';
import { Check } from 'lucide-vue-next';
import type { CheckboxRootEmits, CheckboxRootProps } from 'radix-vue';
import { CheckboxIndicator, CheckboxRoot } from 'radix-vue';
import { type HTMLAttributes } from 'vue';
import { BaseLabel } from '../label';

export type CheckboxOption = {
  label: string;
  description?: string;
  value: string;
  disabled?: boolean;
};

defineOptions({ inheritAttrs: false });
const props = defineProps<
  CheckboxRootProps & {
    class?: HTMLAttributes['class'];
    options: CheckboxOption[];
    modelValue: string[];
    id?: string;
  }
>();

const emits = defineEmits<
  CheckboxRootEmits & {
    'update:modelValue': [value: string[]];
  }
>();

const onChange = (value: string) => {
  let oldValues = props.modelValue;
  if (oldValues.includes(value)) {
    oldValues = oldValues.filter((item) => item !== value);
  } else {
    oldValues.push(value);
  }
  emits('update:modelValue', oldValues);
};
</script>

<template>
  <div :class="cn('flex select-none gap-3', props.class)">
    <BaseLabel
      :class="
        cn('items-top flex cursor-pointer gap-x-1 self-start', {
          'cursor-not-allowed opacity-70': option.disabled,
        })
      "
      v-for="option in options"
      :key="option.value"
    >
      <CheckboxRoot
        :disabled="option.disabled"
        @click="onChange(option.value)"
        :checked="modelValue.includes(option.value)"
        :value="option.value"
        class="peer h-4 w-4 shrink-0 rounded-sm border border-primary ring-offset-background focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 data-[state=checked]:bg-primary data-[state=checked]:text-primary-foreground"
      >
        <CheckboxIndicator class="flex h-full w-full items-center justify-center text-current">
          <Check class="h-4 w-4" />
        </CheckboxIndicator>
      </CheckboxRoot>
      <p class="grid gap-1 text-sm leading-none">
        <span class="flex min-h-4 items-end font-medium">{{ option.label }}</span>
        <span
          class="font-light text-muted-foreground"
          v-if="!!option.description"
        >
          {{ option.description }}
        </span>
      </p>
    </BaseLabel>
  </div>
</template>