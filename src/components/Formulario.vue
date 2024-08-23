<script setup>
    import { reactive } from 'vue'; 


    import Alerta from './Alerta.vue';

    const alerta = reactive({
        mensaje: '',
        tipo: ''
    })

    const emit = defineEmits(['update:nombre','update:propietario','update:email','update:alta','update:sintomas','add-paciente'])

    const props = defineProps({
        nombre:{
            type: String,
            required: true
        },
        propietario:{
            type: String,
            required: true
        },
        email:{
            type: String,
            required: true
        },
        alta:{
            type: String,
            required: true
        },
        sintomas:{
            type: String,
            required: true
        }
    })

    const validar = e => {
        if(Object.values(props).includes('')){
            alerta.mensaje = 'Debes de llenar todos los campos'
            alerta.tipo = 'error'
            console.log('vacios')
            return
        }
        
        emit('add-paciente')
        alerta.mensaje = 'Datos guardados correctamente'
        alerta.tipo = "success"

        setTimeout(() => {
            Object.assign(alerta, {
                mensaje: '',
                tipo: ''
            })
        }, 2500)
    }

    // const nombre = ref('');
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>

        <Alerta v-if="alerta.mensaje"
            :alerta="alerta"
        />

        <form
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validar"
            
            >
            <div class="mb-5">
                <label for="mascota"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre mascota
                </label>
                <input
                    id="mascota"
                    type="text"
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:nombre', $event.target.value)"
                    :value="nombre"
                />
            </div>
            <div class="mb-5">
                <label for="propietario"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre propietario
                </label>
                <input
                    id="propietario"
                    type="text"
                    placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:propietario', $event.target.value)"
                    :value="propietario"
                />
            </div>
            <div class="mb-5">
                <label for="email"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input
                    id="email"
                    type="email"
                    placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:email', $event.target.value)"
                    :value="email"
                />
            </div>
            <div class="mb-5">
                <label for="alta"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Fecha de alta
                </label>
                <input
                    id="alta"
                    type="date"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:alta', $event.target.value)"
                    :value="alta"
                />
            </div>
            <div class="mb-5">
                <label for="sintomas"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Síntomas
                </label>
                <textarea
                    id="sintomas"
                    placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    @input="$emit('update:sintomas', $event.target.value)"
                    :value="sintomas"
                >
                </textarea>
            </div>
            <input type="submit" class="bg-indigo-600 w-full cursor-pointer text-white uppercase font-bold hover:bg-indigo-700 transition-colors p-3" value="registrar paciente">
        </form>
    </div>
</template>


