
<template>
    <div class="container">
        <div class="calendar">
            <VCalendar :attributes="attributes" locale="es" :min-date="minDate" @dayclick="handleDayCLick"></VCalendar>
        </div>
        <div class="reserva"><samp>{{  elementoSeleccionado  }}</samp></div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const elementoSeleccionado = ref();
const minDate = ref(new Date())
const maximasReservasPorDia = 3;
const reservas = ref([
    {
        dates: new Date(2023, 5, 3),
        mesasOcupadas: 2

    },
    {
        dates: new Date(2023, 5, 10),
        mesasOcupadas: 1
    }
])


const handleDayCLick = (calendarDay)=>{
   elementoSeleccionado.value = calendarDay.attributes[0].customData //Esto lo saqué de la documentación del calendario.
   
    
}
const getColorSegunMesasLibres = (mesasLibres) => {
    return mesasLibres === 1 ? 'red' : 'yellow';
}

const attributes = computed(() => [
    ...reservas.value.map(reserva => {
        const mesasLibres = maximasReservasPorDia - reserva.mesasOcupadas;
        return {

            content: getColorSegunMesasLibres(mesasLibres),
            dates: reserva.dates,
            popover: {
                label: `${mesasLibres} mesas libres`
            },
            customData:{ //aca pueden poner lo que necesiten
                mesasLibres,
                elementoSeleccionado:reserva.dates
            }
        }
    })
])


</script>


<style>
.container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
    gap: 5px 5px;
    grid-template-areas:
        "calendar reserva";
}

.calendar {
    grid-area: calendar;
}

.reserva {
    grid-area: reserva;
}
</style>
