<template>
  <form @submit.prevent="handleSubmit" class="mb-4">
    <div class="row g-3">
      <div class="col-md-6">
        <input
          v-model="form.name"
          placeholder="Nombre"
          class="form-control"
          required
          type="text"
          pattern="[A-Za-z\s]+" 
          title="Solo letras y espacios"
        />
      </div>
      <div class="col-md-6">
        <input
          v-model="form.email"
          placeholder="Email"
          class="form-control"
          required
          type="email"
        />
      </div>
      <div class="col-md-6">
        <input v-model="form.address" placeholder="Dirección" class="form-control" type="text" />
      </div>
      <div class="col-md-6">
        <input
          v-model="form.phone"
          placeholder="Teléfono"
          class="form-control"
          type="tel"
          pattern="[\d\-+\s]+"
          title="Solo números, espacios, guiones o '+'"
        />
      </div>
      <div class="col-md-6">
        <input v-model="form.country" placeholder="País" class="form-control" type="text" />
      </div>
      <div class="col-md-6">
        <input v-model="form.city" placeholder="Ciudad" class="form-control" type="text" />
      </div>
    </div>
    <button class="btn btn-success mt-3" type="submit">
      {{ isEdit ? 'Actualizar' : 'Agregar' }}
    </button>
  </form>
</template>

<script setup>
import { ref, watch, defineEmits, defineProps } from 'vue'

const props = defineProps({
  contactoSeleccionado: Object
})

const emit = defineEmits(['save-contact'])

const form = ref({
  id: null,
  name: '',
  email: '',
  address: '',
  phone: '',
  country: '',
  city: ''
})

const isEdit = ref(false)

watch(
  () => props.contactoSeleccionado,
  (newVal) => {
    if (newVal) {
      form.value = { ...newVal }
      isEdit.value = true
    } else {
      resetForm()
    }
  },
  { immediate: true }
)

function handleSubmit() {
  emit('save-contact', { ...form.value })
  resetForm()
}

function resetForm() {
  form.value = {
    id: null,
    name: '',
    email: '',
    address: '',
    phone: '',
    country: '',
    city: ''
  }
  isEdit.value = false
}
</script>
