<template>
  <header class="bg-indigo-600">
    <div class="container mx-auto h-20 flex justify-between items-center text-white">
      <NuxtLink :to="{ name: 'index' }" class="inline-flex items-center gap-x-5">
        <SvgIcon name="icons/logo" class="w-10 h-10" />
        Task editor
      </NuxtLink>
      <NuxtLink to="/add" v-if="$route.name === 'index'">
        <button
          class="hover:bg-indigo-600 bg-white text-indigo-600 font-semibold hover:text-white py-2 px-4 border rounded-md">
          Add
        </button>
      </NuxtLink>
    </div>
  </header>
</template>
