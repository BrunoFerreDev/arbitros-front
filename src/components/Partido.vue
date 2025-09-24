<template>
    <div class="max-w-7xl mx-auto p-6 bg-gray-100 rounded-2xl shadow-lg min-h-[100vh]">
        <!-- Header -->
        <h2 class="text-2xl font-bold text-center mb-6">Panel del Árbitro</h2>
        <p class="text-center text-gray-500 mb-6">
            Partido: {{ partido.local.nombre }} vs {{ partido.visitante.nombre }} | Fecha: {{ partido.fecha }}
        </p>

        <!-- Tabs principales -->
        <div class="flex border-b mb-6">
            <button v-for="tab in tabs" :key="tab" @click="activeTab = tab"
                class="flex-1 py-2 text-center font-semibold"
                :class="activeTab === tab ? 'border-b-4 border-blue-500 text-blue-600' : 'text-gray-500'">
                {{ tab }}
            </button>
        </div>

        <!-- LOCAL -->
        <div v-if="activeTab === 'Local'">
            <div class="p-4 bg-white rounded-2xl shadow-md max-w-5xl mx-auto">
                <div class="mb-6 mt-2 border-b-2 border-gray-200 pb-4">
                    <div class="flex justify-between items-center bg-gray-300 p-4 rounded-lg mb-2 cursor-pointer"
                        @click="abiertoCT = !abiertoCT">
                        <!-- Título -->
                        <h2 class="text-lg font-semibold">⚽ Cuerpo técnico</h2>
                        <span>
                            <IconArrowUp v-if="abiertoCT" />
                            <IconArrowDown v-else />
                        </span>
                    </div>
                    <!-- Grid de dos columnas -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6" v-if="abiertoCT">
                        <!-- Columna izquierda -->
                        <!-- <div class="space-y-3"> -->
                        <FormField label="Director técnico" v-model="ctLocal.directorTecnico" />
                        <FormField label="Entrenador asistente" v-model="ctLocal.entrenadorAsistente" />
                        <FormField label="Preparador físico" v-model="ctLocal.preparadorFisico" />
                        <FormField label="Médico o enfermero" v-model="ctLocal.medico" />
                        <!-- <FormField label="Movil traslado" v-model="form.movilTraslado" /> -->
                        <template v-for="(p, index) in personalExtra" :key="index">
                            <FormField :label="p.label" v-model="p.value" :isExtra="true"
                                @update-label="actualizarLabel(index, $event)" @remove="eliminarPersonal(index)" />
                        </template>
                        <button
                            class="bg-blue-500 hover:bg-blue-600 text-white font-medium px-4 py-2 rounded-lg transition col-span-1 w-[calc(100%-4.5rem)] self-end"
                            @click="agregarPersonal">
                            + Agregar
                        </button>
                        <!-- Botón agregar -->
                        <label for="incidencias" class="flex flex-col gap-2 mt-4 w-3/5 col-span-2">
                            Incidencias
                            <textarea id="incidencias" v-model="incidencias"
                                class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
                                placeholder="Escribe las incidencias" rows="4" cols="50" />
                        </label>
                    </div>
                </div>
                <div class="mb-6 border-gray-200 cursor-pointer" @click="abiertoPersonal = !abiertoPersonal">
                    <div class="flex justify-between items-center bg-gray-300 p-4 rounded-lg mb-2">
                        <h3 class="text-lg font-semibold ">Personal Policial y Movil de Traslado</h3>
                        <IconArrowUp v-if="abiertoPersonal" />
                        <IconArrowDown v-else />
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6" v-if="abiertoPersonal">
                        <FormField label="Nombre Policial" v-model="policial.nombrePolicial" />
                        <FormField label="DNI Policial" v-model="policial.dniPolicial" />
                        <FormField label="Cargo Policial" v-model="policial.cargoPolicial" />
                        <FormField label="Movil de Traslado" v-model="policial.movilTraslado" />
                    </div>
                </div>
            </div>
            <div class="max-w-7xl mx-auto">
                <Alineaciones />
            </div>
        </div>

        <!-- VISITANTE -->
        <div v-if="activeTab === 'Visitante'">
            <div class="p-4 bg-white rounded-2xl shadow-md max-w-5xl mx-auto">
                <div>
                    <div class="flex justify-between items-center bg-gray-300 p-4 cursor-pointer rounded"
                        @click="abiertoCT = !abiertoCT">
                        <!-- Título -->
                        <h2 class="text-lg font-semibold">⚽ Cuerpo técnico</h2>
                        <span>
                            <IconArrowUp v-if="abiertoCT" />
                            <IconArrowDown v-else />
                        </span>
                    </div>
                    <!-- Grid de dos columnas -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 p-5" v-if="abiertoCT">
                        <!-- Columna izquierda -->
                        <!-- <div class="space-y-3"> -->
                        <FormField label="Director técnico" v-model="ctLocal.directorTecnico" />
                        <FormField label="Entrenador asistente" v-model="ctLocal.entrenadorAsistente" />
                        <FormField label="Preparador físico" v-model="ctLocal.preparadorFisico" />
                        <FormField label="Médico o enfermero" v-model="ctLocal.medico" />
                        <!-- <FormField label="Movil traslado" v-model="form.movilTraslado" /> -->
                        <template v-for="(p, index) in personalExtra" :key="index">
                            <FormField :label="p.label" v-model="p.value" :isExtra="true"
                                @update-label="actualizarLabel(index, $event)" @remove="eliminarPersonal(index)" />
                        </template>
                        <button
                            class="bg-blue-500 hover:bg-blue-600 text-white font-medium px-4 py-2 rounded-lg transition col-span-1 w-[calc(100%-4.5rem)] self-end"
                            @click="agregarPersonal">
                            + Agregar
                        </button>
                        <!-- Botón agregar -->
                        <label for="incidencias" class="flex flex-col gap-2 mt-4 w-3/5 col-span-2">
                            Incidencias
                            <textarea id="incidencias" v-model="incidencias"
                                class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
                                placeholder="Escribe las incidencias" rows="4" cols="50" />
                        </label>
                    </div>
                </div>
            </div>
            <div class="max-w-7xl mx-auto">
                <Alineaciones />
            </div>
        </div>

    </div>
</template>

<script setup>
import { computed, ref } from "vue";

import { reactive } from "vue";
import FormField from "./FormField.vue";
import Alineaciones from "./Alineaciones.vue";
import { IconArrowDown, IconArrowUp } from "@tabler/icons-vue";
const abiertoCT = ref(false);
/* -------- DATA -------- */
const tabs = ["Local", "Visitante", "Partido"];
const personalExtra = ref([]);
const abiertoPersonal = ref(false);
const activeTab = ref("Local");

const policial = ref({
    movilTraslado: "",
    nombrePolicial: "",
    dniPolicial: "",
    cargoPolicial: "",
});
const partido = ref({
    fecha: "2025-09-21",
    local: {
        nombre: "Club A",
        titulares: [
            { nombre: "Jugador A1", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
            { nombre: "Jugador A2", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
            { nombre: "Jugador A3", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
        ],
        suplentes: [
            { nombre: "Jugador A4", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
            { nombre: "Jugador A5", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
        ],
        ct: [
            { nombre: "DT Local", amonestado: false, expulsado: false, ficha: "12345678" },
            { nombre: "AT Local", amonestado: false, expulsado: false, ficha: "12345678" },
            { nombre: "PF Local", amonestado: false, expulsado: false, ficha: "12345678" },
        ]
    },
    visitante: {
        nombre: "Club B",
        titulares: [
            { nombre: "Jugador B1", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
            { nombre: "Jugador B2", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
            { nombre: "Jugador B3", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
        ],
        suplentes: [
            { nombre: "Jugador B4", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
            { nombre: "Jugador B5", amonestado: false, expulsado: false, sustitucion: "", ficha: "12345678" },
        ],
        ct: [
            { nombre: "DT Visitante", amonestado: false, expulsado: false, ficha: "12345678" },
            { nombre: "AT Visitante", amonestado: false, expulsado: false, ficha: "12345678" },
            { nombre: "PF Visitante", amonestado: false, expulsado: false, ficha: "12345678" },
        ]
    },
});
const incidencias = ref("");

// Estado del formulario
const ctLocal = reactive({
    directorTecnico: "",
    entrenadorAsistente: "",
    preparadorFisico: "",
    medico: "",
});

// Método para agregar más personal
const agregarPersonal = () => {
    personalExtra.value.push({
        label: `Otro (${personalExtra.value.length + 1})`,
        value: "",
    });
};

// Método para eliminar personal extra
const eliminarPersonal = (index) => {
    personalExtra.value.splice(index, 1);
};
// Método para actualizar label de un extra
const actualizarLabel = (index, nuevo) => {
    personalExtra.value[index].label = nuevo;

};

</script>
<style scoped>
textarea {
    resize: none;
}
</style>