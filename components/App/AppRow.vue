<template>
  <tr>
    <app-cell>
      <template v-for="(_, slot) of $scopedSlots" #[slot]="scope">
      <slot :name="slot" v-bind="scope" />
    </template>
    </app-cell>
  </tr>
</template>
