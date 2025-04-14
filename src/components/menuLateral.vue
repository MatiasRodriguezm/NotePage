<script setup>
const props = defineProps(['nota_activa', 'notas']);
</script>

<template>
    <aside class="w-[300px] min-h-screen bg-gray-950">
        <div class="p-4">
            <button class="button w-full" @click="$emit('nueva-nota')">
                Nueva Nota
            </button>
        </div>
        <ul>
            <li v-for="nota in props.notas" :key="nota.id" @click="$emit('colocar-nota-activa', nota.id)"
                class="p-4 rounded-3xl border border-gray-900 hover:bg-gray-900 cursor-pointer flex flex-col items-start gap-2">

                <span class="break-words w-full">
                    {{ nota.titulo }}
                </span>

                <button class="text-sm text-red-500 cursor-pointer self-start mt-auto hover:bg-red-900 hover:rounded-2xl hover:p-2 hover:text-white"
                    @click.stop="evt => $emit('eliminar-nota', { id: nota.id, evt })">
                    Eliminar
                </button>
            </li>
        </ul>
    </aside>
</template>