<template>
  <v-app>
    <header class="p-4 flex justify-between bg-slate-100">
      <NuxtLink to="/" class="text-2xl font-bold">Ejercicio Postulación</NuxtLink>
      <div>
        <NuxtLink to="/" class="italic font-bold bg-blue-600 text-white p-2 rounded-md mr-2 text-xs">Inicio</NuxtLink>
        <NuxtLink to="/task/create/" class="bg-green-600 p-2 text-white rounded-md text-xs">Agregar nueva tarea</NuxtLink>
      </div>
    </header>
    <slot></slot>
  </v-app>
</template>