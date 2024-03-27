<template>
  <v-container>
    <v-card v-for="task in tasks" :key="task.id" class="md:w-[500px] mx-auto">
      <v-card-title>Detalles de la tarea</v-card-title>
      <v-card-text>
        <v-list >
          <v-list-item>
            <h2 class="font-bold">Titulo:</h2>
            <h3>{{ task.title }}</h3>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item>
            <h2 class="font-bold">Fecha de vencimiento:</h2>
            <h3>{{ task.due_date || 'N/A' }}</h3>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item>
            <h2 class="font-bold">Estado:</h2>
            <h3>{{ task.is_completed ? 'Completado' : 'No completado' }}</h3>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item>
            <h2 class="font-bold">Comentarios:</h2>
            <h3>{{ task.comments || 'N/A' }}</h3>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item>
            <h2 class="font-bold">Descripción:</h2>
            <h3>{{ task.description || 'N/A' }}</h3>
          </v-list-item>

          <v-divider></v-divider>

          <v-list-item>
            <h2 class="font-bold">Tags:</h2>
            <h3>{{ task.tags || 'N/A' }}</h3>
          </v-list-item>
        </v-list>

        <div>
        	<NuxtLink :to="`/task/edit/${task.id}`" class="text-sm bg-yellow-500 text-white py-3 px-3">EDITAR</NuxtLink>
          <v-btn @click="deleteTask" color="error" class="ml-3">Eliminar Tarea</v-btn>
        </div>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script setup>
  const route = useRoute()

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
  
  const api_url = `https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/${route.params.id}`;

  //Al no haber una version de Axios compatible con Nuxt3, toca usar useFetch
  const { data: tasks, error, loading } = useFetch(api_url, request_options, params);


  //Función para eliminar el task de la API
  const deleteTask = async () => {
  try {
    const task_id = route.params.id
    const api_url = `https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/${task_id}`
    
    const response = await fetch(api_url, {
      method: 'DELETE',
      headers: {
        'Authorization': 'Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd'
      }
    })
    
    if (response.ok) {
      console.log('Tarea eliminada con éxito')
      // Redirigir a la página de lista de tareas u otra página relevante
      router.push('/')
    } else {
      console.error('Error al eliminar la tarea:', response.statusText)
    }
  } catch (error) {
    console.error('Error al eliminar la tarea:', error)
  }
}
</script>