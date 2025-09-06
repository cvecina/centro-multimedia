<template>
    <div class="min-h-screen bg-gray-50">
        <!-- Header Component -->
        <AppHeader />

        <!-- Main content -->
        <main class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
            <div class="text-center mb-8">
                <h2 class="text-3xl font-bold text-gray-900 mb-4">
                    Tu Centro de Entretenimiento
                </h2>
                <p class="text-lg text-gray-600 max-w-2xl mx-auto">
                    Accede a todos tus servicios de streaming desde un solo lugar
                </p>
            </div>

            <!-- Servicios multimedia -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 mb-8">
                <!-- Servicios existentes -->
                <div 
                    v-for="service in services" 
                    :key="service.id"
                    @click="openService(service)"
                    class="bg-white rounded-xl shadow-md hover:shadow-lg transition-all duration-300 cursor-pointer group"
                >
                    <div class="p-6 text-center">
                        <div class="w-20 h-20 mx-auto mb-4 rounded-xl flex items-center justify-center text-4xl" :style="{ backgroundColor: service.bgColor }">
                            {{ service.icon }}
                        </div>
                        <h3 class="text-lg font-semibold text-gray-900 mb-2">{{ service.name }}</h3>
                        <p class="text-sm text-gray-600 mb-4">{{ service.description }}</p>
                        <div class="bg-blue-600 text-white px-4 py-2 rounded-lg group-hover:bg-blue-700 transition-colors">
                            Abrir
                        </div>
                    </div>
                </div>

                <!-- Botón para añadir nueva aplicación -->
                <div 
                    @click="openAddServiceModal"
                    class="bg-white rounded-xl shadow-md hover:shadow-lg transition-all duration-300 cursor-pointer group border-2 border-dashed border-gray-300 hover:border-blue-500"
                >
                    <div class="p-6 text-center h-full flex flex-col justify-center">
                        <div class="w-20 h-20 mx-auto mb-4 rounded-xl flex items-center justify-center text-4xl bg-gray-100 group-hover:bg-blue-100 transition-colors">
                            ➕
                        </div>
                        <h3 class="text-lg font-semibold text-gray-600 group-hover:text-blue-600 mb-2">Añadir Servicio</h3>
                        <p class="text-sm text-gray-500">Agrega tu plataforma favorita</p>
                    </div>
                </div>
            </div>



            <!-- Modal para añadir servicio -->
            <div v-if="showAddModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
                <div class="bg-white rounded-xl shadow-2xl w-full max-w-md">
                    <!-- Header del formulario -->
                    <div class="flex items-center justify-between p-6 border-b">
                        <h3 class="text-xl font-bold text-gray-900">Añadir Nuevo Servicio</h3>
                        <button 
                            @click="closeAddModal"
                            class="text-gray-500 hover:text-gray-700 p-2 rounded-lg hover:bg-gray-100 transition-colors"
                        >
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                            </svg>
                        </button>
                    </div>
                    
                    <!-- Formulario -->
                    <form @submit.prevent="addNewService" class="p-6 space-y-4">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Nombre del servicio</label>
                            <input 
                                v-model="newService.name"
                                type="text" 
                                required
                                class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                                placeholder="Ej: Spotify, Twitch, YouTube..."
                            >
                        </div>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Descripción</label>
                            <input 
                                v-model="newService.description"
                                type="text" 
                                required
                                class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                                placeholder="Breve descripción del servicio"
                            >
                        </div>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">URL</label>
                            <input 
                                v-model="newService.url"
                                type="url" 
                                required
                                class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                                placeholder="https://ejemplo.com"
                            >
                        </div>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Icono (emoji)</label>
                            <input 
                                v-model="newService.icon"
                                type="text" 
                                required
                                maxlength="2"
                                class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent text-center text-2xl"
                                placeholder="🎵"
                            >
                        </div>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Color de fondo</label>
                            <input 
                                v-model="newService.bgColor"
                                type="color" 
                                class="w-full h-12 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                            >
                        </div>
                        
                        <div class="flex space-x-3 pt-4">
                            <button 
                                type="button"
                                @click="closeAddModal"
                                class="flex-1 px-4 py-2 text-gray-700 bg-gray-100 rounded-lg hover:bg-gray-200 transition-colors"
                            >
                                Cancelar
                            </button>
                            <button 
                                type="submit"
                                class="flex-1 px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors"
                            >
                                Añadir Servicio
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </main>
    </div>
</template>

<script setup>
import { ref } from 'vue'

// Lista de servicios multimedia
const services = ref([
    {
        id: 1,
        name: 'Amazon Prime Video',
        description: 'Series, películas y contenido original',
        icon: '📺',
        bgColor: '#00A8E1',
        url: 'https://www.primevideo.com'
    },
    {
        id: 2,
        name: 'Disney+',
        description: 'El hogar de Disney, Marvel, Star Wars y más',
        icon: '🏰',
        bgColor: '#113CCF',
        url: 'https://www.disneyplus.com'
    },
    {
        id: 3,
        name: 'Netflix',
        description: 'Entretenimiento ilimitado',
        icon: '🎬',
        bgColor: '#E50914',
        url: 'https://www.netflix.com'
    },
    {
        id: 4,
        name: 'HBO Max',
        description: 'Series y películas premium',
        icon: '👑',
        bgColor: '#9B59B6',
        url: 'https://www.max.com'
    }
])

const showAddModal = ref(false)
const newService = ref({
    name: '',
    description: '',
    url: '',
    icon: '',
    bgColor: '#3B82F6'
})

function openService(service) {
    // Abrir en nueva pestaña en lugar de iframe
    window.open(service.url, '_blank', 'noopener,noreferrer')
    console.log('openService')
}

function openAddServiceModal() {
    showAddModal.value = true
    console.log('openAddServiceModal')
}

function closeAddModal() {
    showAddModal.value = false
    // Limpiar formulario
    newService.value = {
        name: '',
        description: '',
        url: '',
        icon: '',
        bgColor: '#3B82F6'
    }
    console.log('closeAddModal')
}

function addNewService() {
    if (!newService.value.name || !newService.value.url) return
    
    const nextId = Math.max(...services.value.map(s => s.id)) + 1
    
    services.value.push({
        id: nextId,
        name: newService.value.name,
        description: newService.value.description,
        icon: newService.value.icon,
        bgColor: newService.value.bgColor,
        url: newService.value.url
    })
    
    closeAddModal()
    console.log('addNewService')
}

// Cerrar modal con tecla ESC
onMounted(() => {
    const handleEscape = (e) => {
        if (e.key === 'Escape' && showAddModal.value) {
            closeAddModal()
        }
    }
    window.addEventListener('keydown', handleEscape)
    
    onUnmounted(() => {
        window.removeEventListener('keydown', handleEscape)
    })
})
</script>