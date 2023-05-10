<template>
  <div>
    <label
        for="provider"
      >
        Select Provider
        <select
          id="provider"
          v-model="selectedProvider"
          name="provider"
        >
          <option selected>All</option>
          <option>Team A</option>
          <option>Team B</option>
          <option>Team C</option>
        </select>
      </label>

      <div v-for="(item, index) in data" :key="index" >
       <p>{{ item.meta.title }} - {{ item.meta.teams }}</p>
      </div>
    </div>
</template>

<script setup>
const selectedProvider = ref('All')

const filterValue = computed(() => {
  if (selectedProvider.value === 'All') {
    return undefined
  } else {
    return selectedProvider.value
  }
})

const { data } = await useAsyncData(
  `solutions-${selectedProvider}`,
  () =>
    queryContent('/solutions/')
      .only(['meta', '_path'])
      .sort({ title: 1 })
      .where({ meta: { teams: { $in: filterValue.value } } })
      .find(),
  {
    watch: filterValue,
  },
)

</script>