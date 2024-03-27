<template>
  <v-container>
    <div class="md:w-[500px] mx-auto">
      <h1 class="font-bold text-4xl my-2">Crear Nueva Tarea</h1>
      <v-form @submit.prevent="submitForm">
        <v-text-field v-model="task.title" label="Título" required></v-text-field>
        <v-date-picker v-model="task.due_date" label="Fecha de vencimiento" width="450" color="primary" class="mx-auto"></v-date-picker>
        <v-textarea v-model="task.comments" label="Comentarios"></v-textarea>
        <v-textarea v-model="task.description" label="Descripción"></v-textarea>
        <v-text-field v-model="task.tags" label="Tags"></v-text-field>
        <v-checkbox v-model="task.is_completed" label="Completado"></v-checkbox>
        <v-alert v-model="show_warning" type="warning" dense class="mb-4">El campo Título es obligatorio.</v-alert>
        <v-btn type="submit" color="primary">Crear Tarea</v-btn>
      </v-form>
    </div>
  </v-container>
</template>

<script setup>
  const route = useRoute();
  const router = useRouter();

  const headers = {
    'Authorization': 'Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd',
    'Content-Type': 'application/x-www-form-urlencoded'
  };

  const request_options = {
    method: 'POST',
    headers: headers,
  };

  const task = ref({
    title: '',
    is_completed: false,
    due_date: null,
    comments: '',
    description: '',
    tags: ''
  });

  const api_url = 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks';

  const show_warning = ref(false);

  const submitForm = async () => {
    if (!task.value.title) {
      show_warning.value = true;
      return;
    }

    const formData = new URLSearchParams();
    formData.append('title', task.value.title);
    formData.append('is_completed', task.value.is_completed ? '1' : '0');
    formData.append('due_date', task.value.due_date);
    formData.append('comments', task.value.comments);
    formData.append('description', task.value.description);
    formData.append('tags', task.value.tags);

    try {
      const response = await fetch(api_url, {
        ...request_options,
        body: formData
      });

      if (response.ok) {
        console.log('Tarea creada exitosamente');
        console.log(response);
        /*Al parecer estoy teniendo un problema con el POST , se recibe un response con status de 202, se espera una status de 200*/
        router.push('/');
      } else {
        console.error('Error al crear la tarea:', response.statusText);
      }
    } catch (error) {
      console.error('Error en la solicitud:', error);
    }

    show_warning.value = false;
  }
</script>