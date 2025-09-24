<template>
    <div class="w-full">
        <div class="flex items-center justify-between mb-1">
            <!-- Label normal -->
            <label v-if="!editandoLabel" class="block text-gray-700 text-sm font-medium">
                {{ label }}
                <button v-if="isExtra" class="ml-2 text-blue-500 hover:text-blue-700" @click="startEdit" type="button">
                    ✏️
                </button>
            </label>

            <!-- Input editable -->
            <div v-else class="flex items-center gap-2">
                <input ref="labelInput" type="text" v-model="labelTemp" @keydown.enter.prevent="guardarLabel"
                    @blur="guardarLabel" class="border-b border-gray-400 focus:outline-none text-sm px-1" />
                <button class="text-green-600 hover:text-green-800" @click="guardarLabel" type="button"
                    aria-label="Guardar label">
                    ✅
                </button>
            </div>
        </div>
        <div class="flex items-center gap-2">
            <input type="text"
                class="flex-1 border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
                :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" />
            <!-- Botones de acción -->
            <button class="p-1 rounded hover:bg-gray-100">
                <span class="text-gray-500">📋</span>
            </button>
            <button class="p-1 rounded hover:bg-gray-100" v-if="isExtra"
                @click="$emit('remove')">
                <span class="text-red-500">✖</span>
            </button>
            <button class="p-1 rounded hover:bg-gray-100" v-else>
                <span class="text-green-500">✅</span>
            </button>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        label: String,
        modelValue: String,
        isExtra: { type: Boolean, default: false },
    },
    data() {
        return {
            labelInput: null,
            editandoLabel: false,
            labelTemp: String(this.label ?? ''),
        };
    },
    watch: {
        // sincroniza si el prop label cambia desde afuera
        label(nuevo) {
            this.labelTemp = String(nuevo ?? '');
        },
    },
    methods: {
        startEdit() {
            this.editandoLabel = true;
            this.$nextTick(() => {
                if (this.$refs.labelInput) {
                    this.$refs.labelInput.focus();
                    this.$refs.labelInput.select?.();
                }
            });
        },

        guardarLabel() {
            // normalizamos a string y quitamos espacios al inicio/fin
            const nuevo = String(this.labelTemp ?? '').trim();

            // emitimos EL TEXTO (nunca un booleano)
            this.$emit('update-label', nuevo);

            // cerramos edición
            this.editandoLabel = false;
        },
    },
}

</script>