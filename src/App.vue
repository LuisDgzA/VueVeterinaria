<script setup>
	import { ref, reactive, watch, onMounted } from 'vue';

    import { uid } from 'uid';

	import Header from './components/Header.vue';
	import Formulario from './components/Formulario.vue';
	import Paciente from './components/Paciente.vue';

	const pacientes = ref([])


	const paciente = reactive({
		id: null,
		nombre: '',
		propietario: '',
		email: '',
		fechaAlta: '',
		sintomas: '',
	})

	const addPaciente = () => {
		if(paciente.id){
			const { id } = paciente
			const i = pacientes.value.findIndex(pacienteState => pacienteState.id === id)
			pacientes.value[i] = {...paciente}
		}else{

			console.log('agregando')
			pacientes.value.push({
				...paciente,
				id: uid()
			})
		}
		paciente.nombre= ''
		paciente.propietario= ''
		paciente.email= ''
		paciente.fechaAlta= ''
		paciente.sintomas= ''
		paciente.id = null
	}

	const editPaciente = (id) => {
		console.log(id)
		const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]

		Object.assign(paciente, pacienteEditar)
		
	}

	const elimiarPaciente = (id) => {
		pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
	}

	onMounted(() => {
		if(localStorage.getItem('pacientes') != null){
			pacientes.value = JSON.parse(localStorage.getItem('pacientes'))
		}

	});

	watch(pacientes, () => {
		savePacientesLocalStorage()
	},
	{
		deep: true
	})

	const savePacientesLocalStorage = () => {
		localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
	}
</script>

<template>
	<div class="container mx-auto mt-20">
		<Header />
		<div class="mt-12 md:flex">
			<Formulario
				v-model:nombre="paciente.nombre"
				v-model:propietario="paciente.propietario"
				v-model:email="paciente.email"
				v-model:alta="paciente.fechaAlta"
				v-model:sintomas="paciente.sintomas"
				@add-paciente="addPaciente"
				:id="paciente.id"
			/>

			<div class="md:w-1/2 md:h-screen overflow-y-scroll">
				<h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
				<p class="text-lg mt-5 text-center mb-10">
					Información de
					<span class="text-indigo-600 font-bold">pacientes</span>
				</p>
				<div v-if="pacientes.length > 0">
					<Paciente
						v-for="paciente in pacientes"
						:paciente="paciente"
						@edit-paciente="editPaciente"
						@eliminar-paciente="elimiarPaciente"
					/>
				</div>
				<p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
			</div>

		</div>
	</div>
</template>

