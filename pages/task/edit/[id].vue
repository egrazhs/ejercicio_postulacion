<template>
  <v-container>
    <v-card class="w-full md:w-[500px] mx-auto">
      <v-card-title>Editar Tarea</v-card-title>
      <v-card-text>
        <!-- Formulario de edición de tarea -->
        <v-form @submit.prevent="editTask">
          <v-text-field v-model="task[0].title" label="Título"></v-text-field>
          <v-text-field v-model="task[0].due_date" label="Fecha de Vencimiento" type="date"></v-text-field>
          <v-checkbox v-model="task[0].is_completed" label="Completada"></v-checkbox>
          <v-textarea v-model="task[0].description" label="Descripción"></v-textarea>
          <v-text-field v-model="task[0].tags" label="Tags"></v-text-field>
          <v-btn type="submit" color="primary">Guardar Cambios</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script setup>
const route = useRoute()
const router = useRouter()

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
const { data: task, error, loading } = useFetch(api_url, request_options, params);

// Método para enviar la solicitud de edición de la tarea
const editTask = async () => {
  try {
    const response = await fetch(api_url, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd'
      },
      body: JSON.stringify(task)
    })
    if (response.ok) {
      console.log('Tarea editada con éxito')
      console.log(response);
      /*Al parecer, al igual que con las peticiones de POST, con las peticiones PUT tambien recibo response con status 202, se espera 200, trabajando en ello.*/
      router.push(`/task/${route.params.id}`);
    } else {
      console.error('Error al editar la tarea:', response.statusText)
    }
  } catch (error) {
    console.error('Error al editar la tarea:', error)
  }
}
</script>
