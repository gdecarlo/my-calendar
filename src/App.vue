<script setup>
import { ref,computed } from 'vue';

const minDate= ref(new Date())
const reservas = ref([
    {
        descripcion:'Dia del trabajador',
        dates:new Date(2023, 5, 3)
    },
    {
        descripcion:'Otro dia',
        dates:new Date(2023, 5, 10)
    }
])

const attributes = computed(()=>[
    ...reservas.value.map(reserva=>({
        content:'red',
        dates:reserva.dates,
        popover:{
            label:reserva.descripcion
        }
    }))
])


</script>

<template>
   <VCalendar :attributes="attributes" locale="es" :min-date="minDate">
   </VCalendar>
</template>

