<template>
    <table class="mx-auto mb-4 border rounded-lg">
        <thead class="bg-gray-100">
            <tr>
                <th class="p-2 w-16">#</th>
                <th class="p-2 w-64">Nombre</th>
                <th class="p-2 w-64">Ficha/DNI</th>
                <th class="p-2 w-64">Amonestación</th>
                <th class="p-2 w-64">Expulsión</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(c, i) in ct" :key="i" class="border-b">
                <td class="p-2">{{ i + 1 }}</td>
                <td class="p-2">{{ c.nombre }}</td>
                <td class="p-2">{{ c.ficha }}</td>
                <td class="p-2 text-center"><input type="checkbox" class="border rounded p-1 w-6 h-6" v-model="c.amonestado" /></td>
                <td class="p-2 text-center"><input type="checkbox" class="border rounded p-1 w-6 h-6" v-model="c.expulsado" /></td>
            </tr>
        </tbody>
    </table>
</template>
<script setup>
const props = defineProps({
    ct: Array,
});
</script>
<style scoped>
@reference "tailwindcss";

th {
    @apply p-2;
}

td,
th {
    @apply text-center border p-2;
}
</style>