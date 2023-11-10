<template>
  <div>
    <table>
      <thead>
        <tr>
          <th>Header</th>
        </tr>
      </thead>
      <tbody>
          <app-row>
            <template v-for="(_, slot) of $scopedSlots" #[slot]="scope">
              <slot :name="slot" v-bind="scope" />
            </template>
          </app-row>
      </tbody>
    </table>
  </div>
</template>
