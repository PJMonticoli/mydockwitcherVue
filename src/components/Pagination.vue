<script setup>
import { defineProps, defineEmits, ref } from 'vue'

const props = defineProps({
  pagination: {
    type: Object,
    default() {
      return {
        last: true,
        totalPages: 1,
        totalElements: 1,
        first: true,
        size: 10,
        number: 0
      }
    }
  }
})

const emits = defineEmits(['changePage'])

const PAGES_TO_SHOW = 5

function changePage(page) {
  emits('changePage', page)
}
const getPagesToShow = () => {
  // Si el total de páginas es menor a 5, muestra todas las páginas sino solo muestra 5
  const pageRange =
    props.pagination.totalPages < PAGES_TO_SHOW ? props.pagination.totalPages : PAGES_TO_SHOW
  const startPage = Math.max(
    1,
    Math.min(props.pagination.number + 1, props.pagination.totalPages - pageRange + 1)
  )
  return [...Array(pageRange)].map((_, i) => startPage + i)
}

const getPageStyles = (page) => {
  return {
    'background-color': page === props.pagination.number ? 'rgb(246, 55, 93)' : 'transparent',
    color: page === props.pagination.number ? '#fff' : '#333'
  }
}
</script>

<template>
  <div>
    <span
      >De {{ pagination.number * pagination.size + 1 }} a
      {{ Math.min(pagination.number * pagination.size + 1, pagination.totalElements) }} de
      {{ pagination.totalElements }} registros</span
    >
    <nav aria-label="Page navigation">
      <ul class="pagination justify-content-end">
        <!-- Botón para retroceder a la primer página -->
        <li class="page-item">
          <button
            class="page-link"
            @click="changePage(0)"
            :disabled="pagination.first"
            style="border: none; border-radius: 50%; color: #333"
          >
            <font-awesome-icon :icon="['fas', 'backward-step']" style="color: #808080" />
          </button>
        </li>

        <!-- Botón para retroceder una página -->
        <li class="page-item" :class="{ disabled: pagination.first }">
          <button
            class="page-link"
            @click="changePage(pagination.number - 1)"
            :disabled="pagination.first"
            style="border: none; border-radius: 50%; color: #333"
          >
            <font-awesome-icon :icon="['fas', 'chevron-left']" style="color: #808080" />
          </button>
        </li>

        <!-- Mostrar páginas -->
        <li v-for="page in getPagesToShow()" :key="page" class="page-item">
          <button
            class="page-link text-secondary"
            @click="changePage(page - 1)"
            :class="getPageStyles(page - 1)"
            :disabled="page - 1 == pagination.number"
            style="border: none; border-radius: 50%"
          >
            {{ page }}
          </button>
        </li>

        <!-- Botón para avanzar una página -->
        <li class="page-item" :class="{ disabled: pagination.last }">
          <button
            class="page-link"
            @click="changePage(pagination.number + 1)"
            :disabled="pagination.last"
            style="border: none; border-radius: 50%; color: #333"
          >
            <font-awesome-icon :icon="['fas', 'chevron-right']" style="color: #808080" />
          </button>
        </li>

        <!-- Botón para avanzar a la última página -->
        <li class="page-item">
          <button
            class="page-link"
            @click="changePage(totalPages)"
            :disabled="pagination.last"
            style="border: none; border-radius: 50%; color: #333"
          >
            <font-awesome-icon :icon="['fas', 'forward-step']" style="color: #808080" />
          </button>
        </li>
      </ul>
    </nav>
  </div>
</template>
