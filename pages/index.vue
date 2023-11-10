<template>
  <app-table>
    <template #cell="{ item, index }">
      <div>{{ `${item} - ${index}` }}</div>
    </template>
  </app-table>
</template>

<script>
export default {
  methods: {
    booleanToInt(value, parent = null, index = null) {
      const parentIsObject = this.checkIsObject(parent)
      const target = parentIsObject ? parent[value] : value
      if (typeof target === "boolean") {
        if (parent === null) {
          return +target
        }
        if (index === null) {
          parent[value] = +target
        } else {
          parent[index] = +target
        }
        return
      }

      const isArray = this.checkIsArray(target)
      if (isArray) {
        target.forEach((el, idx) => {
          this.booleanToInt(el, target, idx)
        })
      }

      const isObject = this.checkIsObject(target)
      if (isObject) {
        for (const item in target) {
          this.booleanToInt(item, target)
        }
      }
    },

    checkIsArray(value) {
      return typeof value === 'object' && value !== null && Array.isArray(value)
    },

    checkIsObject(value) {
      return typeof value === 'object' && value !== null && !Array.isArray(value)
    },

    copy(targetObj, pathArray) {
      const copyObj = {}
      const cloneMap = {
        original: targetObj,
        copyObj: copyObj,
      }
      pathArray.forEach((path) => {
        const splittedPath = path.split('.')
        const hasValue = splittedPath.reduce((acc, prop) => acc?.[prop], targetObj)
        if (!hasValue) {
          return
        }
        splittedPath.forEach((prop, idx) => {
          if (idx === splittedPath.length - 1) {
            const value = cloneMap.original[prop]
            cloneMap.copyObj[prop] = value
            return
          }
          const isArray = this.checkIsArray(cloneMap.original[prop])
          if (isArray && !cloneMap.copyObj[prop]) {
            cloneMap.copyObj[prop] = []
          }
          const isObject = this.checkIsObject(cloneMap.original[prop])
          if (isObject && !cloneMap.copyObj[prop]) {
            cloneMap.copyObj[prop] = {}
          }
          cloneMap.original = cloneMap.original[prop]
          cloneMap.copyObj = cloneMap.copyObj[prop]
        })
        cloneMap.original = targetObj
        cloneMap.copyObj = copyObj
      })
      console.log('copy', copyObj)
      console.log('original', targetObj)
      return copyObj
    },
  },

  mounted() {
    const test1 = {
    date1: {
      date1_1: 1,
      date1_2: [
        {
          date2_1: false,
          date2_2: 'str1',
        },
        {
          date2_3: true,
          date2_4: 'str2',
        },
        {
          date2_5: false,
          date2_6: 'str1',
        },
      ],
      date1_3: false,
      date1_4: {
        date3_1: true,
        date3_2: false,
        date3_3: 'str1',
        date3_4: 123,
      },
      date1_5: 'true',
    }
}
    const test2 = { b: { c: 3, d: [3, 4] }, a: 12 }
    this.booleanToInt(test1)
    console.log('result booleanToInt', test1)
    this.copy(test2, ['a.a', 'b.c', 'b.d.0', 'b.c.e'])
  }
}
</script>
