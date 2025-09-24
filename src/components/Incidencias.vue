<template>
  <div class="border rounded-lg p-4" id="incidencias">
    <h5 class="text-xl font-semibold text-gray-500">{{ club }}</h5>
    <!-- Amonestados -->
    <div>
      <h3 class="text-xl font-semibold text-yellow-600 mb-2">Amonestados</h3>
      <table class="w-full border rounded-lg">
        <thead class="bg-gray-100">
          <tr>
            <th class="p-2 text-left">Nombre</th>
            <th class="p-2 text-left">Equipo</th>
            <th class="p-2 text-left">Motivo</th>
            <th class="p-2 text-left">Minutos</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(p, i) in amonestados" :key="'amonest-' + i" class="border-b">
            <td class="p-2">{{ p.nombre }}</td>
            <td class="p-2">{{ p.equipo }}</td>
            <td class="p-2">
              <select name="" id="" v-model="p.motivo">
                <option value="">Motivo 1</option>
                <option value="">Motivo 2</option>
                <option value="">Motivo 3</option>
              </select>
            </td>
            <td class="p-2">
              <input v-model="p.minutos" type="number" placeholder="Minutos" class="w-24 border rounded p-1" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Expulsados -->
    <div>
      <h3 class="text-xl font-semibold text-red-600 mb-2">Expulsados</h3>
      <table class="w-full border rounded-lg">
        <thead class="bg-gray-100">
          <tr>
            <th class="p-2 text-left">Nombre</th>
            <th class="p-2 text-left">Equipo</th>
            <th class="p-2 text-left">Motivo</th>
            <th class="p-2 text-left">Minutos</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(p, i) in expulsados" :key="'expuls-' + i" class="border-b">
            <td class="p-2">{{ p.nombre }}</td>
            <td class="p-2">{{ p.equipo }}</td>
            <td class="p-2">
              <select name="" id="" v-model="p.motivo">
                <option value="">Motivo 1</option>
                <option value="">Motivo 2</option>
                <option value="">Motivo 3</option>
              </select>
            </td>
            <td class="p-2">
              <input v-model="p.minutos" type="number" placeholder="Minutos" class="w-24 border rounded p-1" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Sustituciones -->
    <div>
      <h3 class="text-xl font-semibold text-blue-600 mb-2">Sustituciones</h3>
      <table class="w-full border rounded-lg">
        <thead class="bg-gray-100">
          <tr>
            <th class="p-2 text-left">Equipo</th>
            <th class="p-2 text-left">Jugador que sale</th>
            <th class="p-2 text-left">Jugador que entra</th>
            <th class="p-2 text-left">Minutos</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(s, i) in sustituciones" :key="'sust-' + i" class="border">
            <td>{{ i + 1 }}</td>
            <td>{{ s.equipo }}</td>
            <!-- SALE -->
            <td>
              <select v-model="s.sale" class="w-full border rounded p-1">
                <option disabled value="">-- Seleccionar --</option>
                <option v-for="t in s.titulares" :key="t.ficha" :value="t.nombre">
                  {{ t.nombre }}
                </option>
              </select>
            </td>
            <!-- ENTRA -->
            <td>
              <select v-model="s.entra" class="w-full border rounded p-1">
                <option disabled value="">-- Seleccionar --</option>
                <option v-for="sup in s.suplentes" :key="sup.ficha" :value="sup.nombre">
                  {{ sup.nombre }}
                </option>
              </select>
            </td>
            <!-- Minuto -->
            <td>
              <input v-model="s.minuto" type="number" placeholder="Min" class="w-full border rounded p-1" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  amonestados: Array,
  expulsados: Array,
  sustituciones: Array,
  club: String,
});
</script>
<style>
#incidencias {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  gap: 2rem;
  align-self: flex-start;
}
</style>