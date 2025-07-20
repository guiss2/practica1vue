<template>
  <table class="table table-bordered table-hover">
    <thead class="table-dark">
      <tr>
        <th>Nombre</th>
        <th>Email</th>
        <th>Dirección</th>
        <th>Teléfono</th>
        <th>País</th>
        <th>Ciudad</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="contact in contacts" :key="contact.id">
        <td>{{ contact.name }}</td>
        <td>{{ contact.email }}</td>
        <td>{{ contact.address }}</td>
        <td>{{ contact.phone }}</td>
        <td>{{ contact.country }}</td>
        <td>{{ contact.city }}</td>
        <td>
          <button class="btn btn-warning btn-sm me-2" @click="$emit('edit-contact', contact)">Editar</button>
          <button class="btn btn-danger btn-sm" @click="$emit('delete-contact', contact.id)">Eliminar</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
defineProps({
  contacts: Array
})
</script>
