<template>
    <div class="flex flex-col max-w-5xl mx-auto gap-4 mt-5 shadow-lg bg-white p-6 rounded-2xl">
        <!-- Jugadores disponibles -->
        <div class="shadow-md rounded">
            <div class="flex items-center cursor-pointer bg-blue-200 justify-between rounded" @click="abierto = !abierto">
                <h3 class="p-4 font-semibold">Jugadores disponibles</h3>
                <span class="px-4">
                    <IconArrowUp v-if="abierto" />
                    <IconArrowDown v-else />
                </span>
            </div>

            <table class="border w-full" v-if="abierto">
                <thead>
                    <tr>
                        <th>Núm.</th>
                        <th>N° Doc</th>
                        <th>Jugador</th>
                        <th>T</th>
                        <th>S</th>
                        <th>POR</th>
                        <th>CP</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(jugador, index) in jugadores" :key="jugador.doc">
                        <td>{{ index + 1 }}</td>
                        <td>{{ jugador.doc }}</td>
                        <td>{{ jugador.nombre }}</td>
                        <td><input type="checkbox" @change="agregarATitulares(jugador.doc)"
                                :disabled="verificarCantidadTitulares" /></td>
                        <td><input type="checkbox" @change="agregarASuplentes(jugador.doc)"
                                :disabled="verificarCantidadTitulares == false || jugador.esTitular" /></td>
                        <td><input type="checkbox" @change="agregarAArquero(jugador.doc)"
                                :disabled="!verificarCantidadArquero" /></td>
                        <td><input type="checkbox" @change="agregarACapitan(jugador.doc)" :checked="jugador.esCapitan"
                                :disabled="verificarCantidadCapitan" /></td>
                    </tr>
                </tbody>
            </table>
        </div>
        <!-- Alineación -->
        <div class=" shadow-md rounded">
            <div class="flex items-center cursor-pointer bg-blue-200 justify-between rounded" @click="abiertoAI = !abiertoAI">
                <h3 class=" px-4 py-4 font-semibold">Alineación: {{ alineacion.length }} / Alineación
                    inicial: {{ titularesCount }}</h3>
                <span class="px-4">
                    <IconArrowUp v-if="abiertoAI" />
                    <IconArrowDown v-else />
                </span>
            </div>
            <table class="w-full border" v-if="abiertoAI">
                <thead>
                    <tr>
                        <th>Núm.</th>
                        <th>N° Doc</th>
                        <th>Jugador</th>
                        <th colspan="2">Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-if="alineacion.length === 0">
                        <td colspan="4" class="text-center">No records found.</td>
                    </tr>
                    <tr v-for="(jugador, index) in alineacion" :key="jugador.doc">
                        <td>{{ index + 1 }}</td>
                        <td>{{ jugador.doc }}</td>
                        <td>{{ jugador.nombre }} | {{ jugador.esCapitan ? 'CP' : jugador.esArquero ? 'POR' : '' }}</td>
                        <td><button
                                class="bg-blue-500 hover:bg-blue-600 text-white font-medium px-2 py-1 rounded-lg transition cursor-pointer">Evento</button>
                        </td>
                        <td><button @click="eliminarJugador(jugador)"
                                class="bg-blue-500 hover:bg-blue-600 text-white font-medium px-2 py-1 rounded-lg transition cursor-pointer">Eliminar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script setup>
import { IconArrowUp } from '@tabler/icons-vue'
import { IconArrowDown } from '@tabler/icons-vue'
import { ref, computed } from 'vue'
const abierto = ref(false)
const abiertoAI = ref(false)
const jugadores = ref([
    { doc: Math.floor(Math.random() * 1000), nombre: 'ROMERO, MARIANO MARTIN', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'YBARRA, ARNALDO LEONEL', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'MORA GARRONE, FACUNDO', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'ROMERO, MARIANO MARTIN', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'YBARRA, ARNALDO LEONEL', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'MORA GARRONE, FACUNDO', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'ROMERO, MARIANO MARTIN', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'YBARRA, ARNALDO LEONEL', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'MORA GARRONE, FACUNDO', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'ROMERO, MARIANO MARTIN', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'YBARRA, ARNALDO LEONEL', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'MORA GARRONE, FACUNDO', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'ROMERO, MARIANO MARTIN', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'YBARRA, ARNALDO LEONEL', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'MORA GARRONE, FACUNDO', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'ROMERO, MARIANO MARTIN', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'YBARRA, ARNALDO LEONEL', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
    { doc: Math.floor(Math.random() * 1000), nombre: 'MORA GARRONE, FACUNDO', esTitular: false, esSuplente: false, esCapitan: false, esArquero: false },
])

const alineacion = ref([])//limite de 18
const maxTitulares = ref(11) // Límite configurable de titulares

const agregarATitulares = (jugadorId) => {
    const existeJugador = alineacion.value.find(j => j.doc === jugadorId);
    const jugador = jugadores.value.find(j => j.doc === jugadorId);
    if (existeJugador) {
        return
    } else {
        jugador.esTitular = true;
        alineacion.value.push(jugador);

    }
}

const agregarASuplentes = (jugadorId) => {
    const jugador = jugadores.value.find(j => j.doc === jugadorId);
    if (!jugador) return;
    jugador.esSuplente = true;
    alineacion.value.push(jugador);
}
const agregarACapitan = (jugadorId) => {
    if (verificarCantidadCapitan.value) {
        alert('Ya se agrego un capitan')
        return
    }
    const jugador = jugadores.value.find(j => j.doc === jugadorId);
    if (!jugador) return;
    jugador.esCapitan = true;
    alineacion.value.push(jugador);
}
const agregarAArquero = (jugadorId) => {
    if (verificarCantidadArquero.value) {
        const jugador = alineacion.value.find(j => j.doc === jugadorId);
        jugador.esArquero = true;
        alineacion.value.push(jugador);
        return
    } else {
        alert('Ya se agregaron dos arqueros')
        return
    }
}

const eliminarJugador = (jugador) => {
    alineacion.value = alineacion.value.filter(j => j.doc !== jugador.doc);
}

const titularesCount = computed(() =>
    alineacion.value.filter(j => j.esTitular).length
)
const esTitular = (jugador) => jugador.esTitular
const esSuplente = (jugador) => jugador.esSuplente
const esCapitan = (jugador) => jugador.esCapitan
const esArquero = (jugador) => jugador.esArquero

const verificarCantidadTitulares = computed(() => {
    if (alineacion.value.filter(j => j.esTitular).length >= maxTitulares.value) {
        return true
    }
    return false
})
const verificarCantidadCapitan = computed(() => {
    if (alineacion.value.filter(j => j.esCapitan).length == 1) {
        return true
    }
    return false
})
const verificarCantidadArquero = computed(() => {
    if (alineacion.value.filter(j => j.esArquero).length < 3) {
        return true
    }
})
</script>

<style scoped>
table,
th,
td {
    border: 1px solid #ccc;
    border-collapse: collapse;
    padding: 8px;
    text-align: center;
}
</style>