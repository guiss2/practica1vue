<template>
  <div class="container mt-4">
    <h1 class="mb-4">Catálogo de Contactos</h1>

    <div v-if="mensaje" class="alert alert-success" role="alert">
      {{ mensaje }}
    </div>

    <input
      v-model="busqueda"
      type="text"
      class="form-control mb-3"
      placeholder="Filtrar por nombre o correo electrónico"
    />
    <ContactForm
      :contactoSeleccionado="contactoSeleccionado"
      @save-contact="guardarContacto"
    />
    <ContactList
      :contacts="contactosFiltrados"
      @edit-contact="editarContacto"
      @delete-contact="eliminarContacto"
    />
  </div>
</template>
<script setup>
import { ref, computed } from 'vue'
import ContactList from '../components/ContactList.vue'
import ContactForm from '../components/ContactForm.vue'

const contactos = ref([
  {
    id: 1,
    name: "Alice Johnson",
    email: "alice.johnson@example.com",
    address: "123 Maple Street",
    phone: "123-456-7890",
    country: "USA",
    city: "New York"
  },
  {
    id: 2,
    name: "Bob Smith",
    email: "bob.smith@example.com",
    address: "456 Oak Avenue",
    phone: "987-654-3210",
    country: "Canada",
    city: "Toronto"
  },
  {
    id: 3,
    name: "Carol White",
    email: "carol.white@example.com",
    address: "789 Pine Road",
    phone: "555-123-4567",
    country: "UK",
    city: "London"
  },
  {
    id: 4,
    name: "David Brown",
    email: "david.brown@example.com",
    address: "321 Elm Street",
    phone: "444-555-6666",
    country: "Australia",
    city: "Sydney"
  },
  {
    id: 5,
    name: "Emily Davis",
    email: "emily.davis@example.com",
    address: "654 Spruce Lane",
    phone: "333-444-5555",
    country: "USA",
    city: "Los Angeles"
  }
])

const contactoSeleccionado = ref(null)
const mensaje = ref('')
const busqueda = ref('')

let nextId = contactos.value.length + 1

const contactosFiltrados = computed(() => {
  const q = busqueda.value.toLowerCase().trim()
  if (!q) return contactos.value
  return contactos.value.filter(c =>
    c.name.toLowerCase().includes(q) || c.email.toLowerCase().includes(q)
  )
})

function guardarContacto(contacto) {
  if (contacto.id) {
    // Editar
    const index = contactos.value.findIndex(c => c.id === contacto.id)
    if (index !== -1) {
      contactos.value[index] = { ...contacto }
      mensaje.value = 'Contacto actualizado correctamente.'
    }
  } else {
    // Agregar
    contacto.id = nextId++
    contactos.value.push(contacto)
    mensaje.value = 'Contacto agregado exitosamente.'
  }
  contactoSeleccionado.value = null
  limpiarMensaje()
}

function editarContacto(contacto) {
  contactoSeleccionado.value = { ...contacto }
}

function eliminarContacto(id) {
  contactos.value = contactos.value.filter(c => c.id !== id)
  mensaje.value = 'Contacto eliminado correctamente.'
  limpiarMensaje()
}

function limpiarMensaje() {
  setTimeout(() => {
    mensaje.value = ''
  }, 3000) 
}
</script>
