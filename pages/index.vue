<template>
  <div>
    <div class="flex justify-between">
      <h1 class="font-bold text-2xl px-4 mt-4">Lista de Tareas</h1>
    </div>
    <v-divider color="#000" thickness="2" opacity="0"></v-divider>

    <v-list class="mx-auto w-full md:w-[500px]">
      <template v-for="task in tasks" :key="task.id">
        <v-list-item>
          <div class="flex bg-slate-50 hover:pl-[4px] p-1">
            <div class="w-[80%] flex">
              <div>
                <v-checkbox v-model="task.is_completed" color="success" hide-details disabled class="!inline" @change="updateTask(index)"/>
              </div>
              <div>
                <!--{{task}}-->
                <v-list-item-title class="!font-bold !text-[15px] mt-[18px] font-size-[50px] text-slate-600">{{ task.title }}</v-list-item-title>
                <v-list-item-subtitle v-if="task.due_date" class="!text-[9px] italic mt-1">Fecha de Vencimiento: {{ task.due_date }}</v-list-item-subtitle>
              </div>
            </div>
            <div class="w-[20%]">
              <v-chip size="x-small" class="my-2" :color="task.is_completed ? 'success' : 'error'">
                <template #default>
                  {{ task.is_completed ? 'Completado' : 'No Completado' }}
                </template>
              </v-chip>
              <div>
                <NuxtLink :to="`/task/${task.id}`" class="text-[10px] italic bg-blue-600 text-white text-center rounded-xl p-1">Ver detalles</NuxtLink>
              </div>
            </div>
          </div>
        </v-list-item>
        <v-divider thickness="1" class="!my-0"></v-divider>
      </template>
    </v-list>
  </div>
</template>

<script setup>
  const headers = {
      'Authorization': 'Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd',
  };

  const request_options = {
    method: 'GET',
    headers: headers,
  };

  const params = {
    token: '',
  };

  const api_url = 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks';

  //Al no haber una version de Axios compatible con Nuxt3, toca usar useFetch
  const { data: tasks, error, loading } = useFetch(api_url, request_options, params);
</script>
