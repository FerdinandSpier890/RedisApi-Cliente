<template>
    <ion-modal :is-open="nuevaTarea" @dismiss="cerrarFormulario">
        <ion-header>
            <ion-toolbar style="background-color: #4b662d;">
                <ion-title>Nueva Tarea</ion-title>
                <ion-buttons slot="end">
                    <ion-button @click="cerrarFormulario">
                        <ion-icon name="close"></ion-icon>
                    </ion-button>
                </ion-buttons>
            </ion-toolbar>
        </ion-header>

        <ion-content style="background-color: #20262E">
            <ion-card>
                <ion-card-content>
                    <ion-item>
                        <ion-label position="floating" style="color: #FFFFFF; font-size: 18px;">Nombre de la Tarea
                        </ion-label>
                        <ion-input v-model="nombreTarea" required style="font-size: 18px;"></ion-input>
                    </ion-item>
                    <ion-item>
                        <ion-label position="floating" style="color: #FFFFFF; font-size: 18px;">Descripcion de la Tarea</ion-label>
                        <ion-input v-model="descripcionTarea" required style="font-size: 18px;"></ion-input>
                    </ion-item>
                </ion-card-content>
            </ion-card>
            <ion-footer style="background-color: #4b662d;">
                <ion-toolbar>
                    <ion-buttons slot="start">
                        <ion-button color="white" @click="cerrarFormulario"
                            style="background-color: #263A29; border-radius: 10px; font-size: 18px;">
                            <i class="fas fa-times fa-xl" style="margin-right: 10px;"></i> Cancelar
                        </ion-button>
                    </ion-buttons>
                    <ion-buttons slot="end">
                        <ion-button color="white" @click="guardarTarea"
                            style="background-color: #263A29; border-radius: 10px; font-size: 18px;">
                            <i class="fas fa-save fa-xl" style="margin-right: 10px;"></i> Guardar
                        </ion-button>
                    </ion-buttons>
                </ion-toolbar>
            </ion-footer>
        </ion-content>
    </ion-modal>
</template>
  
<script>
import { IonCard, IonCardHeader, IonCardTitle, IonCardContent, IonButton, IonItem, IonLabel, IonInput, IonModal, IonToast } from '@ionic/vue';
import { defineComponent } from 'vue';
import swal from 'sweetalert';

export default defineComponent({
    components: {
        IonCard,
        IonCardHeader,
        IonCardTitle,
        IonCardContent,
        IonButton,
        IonItem,
        IonLabel,
        IonInput,
        IonModal,
        IonToast
    },
    props: {
        nuevaTarea: {
            type: Boolean,
            required: true
        },
        formTitle: {
            type: String,
            required: true
        },
    },
    data() {
        return {
            nombreTarea: '',
            descripcionTarea: '',
            fechaCreacion: '',
            task: [],
        };
    },
    methods: {
        cerrarFormulario() {
            this.$emit('dismiss');
            this.nuevaTarea = false;
            this.nombreTarea = '';
            this.descripcionTarea = '';
            this.resetFormulario()
        },

        resetFormulario() {
            this.nombreTarea = '';
            this.descripcionTarea = '';
        },

        async guardarTarea() {

            if (this.validarCampos()) {

                function generateGuid() {
                    let d = Date.now();
                    if (typeof performance !== 'undefined' && typeof performance.now === 'function') {
                        d += performance.now(); // Agregar tiempo de alta resolución si está disponible
                    }
                    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                        const r = (d + Math.random() * 16) % 16 | 0;
                        d = Math.floor(d / 16);
                        return (c === 'x' ? r : (r & 0x3 | 0x8)).toString(16);
                    });
                };

                const tarea = {
                    id: generateGuid(),
                    nombreTarea: this.nombreTarea,
                    descripcionTarea: this.descripcionTarea,
                    fechaCreacion: new Date().toISOString()
                };

                try {
                    const response = await fetch("https://localhost:44305/api/Tareas", {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json",
                        },
                        body: JSON.stringify(tarea),
                    });

                    if (response.ok) {
                        //location.reload();
                        //const sound = new Audio('../sounds/xp.mp3');
                        //sound.play();
                        swal("Registro Exitoso", "La Tarea Se Creó Correctamente", "success");
                        setTimeout(() => {
                            location.reload();
                        }, 2000);
                        this.cerrarFormulario();
                        this.$emit('actualizarTareas');
                        this.$emit('dismiss'); // Emitir evento 'dismiss' después de cerrar el formulario
                        this.mostrarTareas(); // Actualizar la lista de productos

                        //location.reload();
                    } else {
                        swal("Error", "No Se Puede Crear la Tarea", "error");
                    }

                    console.log(response);
                    this.nuevaTarea = false;
                    this.resetFormulario();
                    this.$emit('actualizarTareas');
                    this.cerrarFormulario();
                    this.mostrarTareas(); // Actualizar la lista de productos
                } catch (error) {
                    swal("Error", error, "error");
                }
            }
        },
        async mostrarTareas() {
            try {
                const response = await fetch('https://localhost:44305/api/Tareas');
                if (!response.ok) {
                    swal("Error", "No Se Puede Mostrar Las Tareas", "error")
                }
                const data = await response.json();
                this.task = data;
            } catch (error) {
                swal("Error", error, "error")
            }
        },
        actualizarTareas() {
            this.mostrarTareas();
        },
        validarCampos() {
            if (this.nombreTarea.trim() === "" ||
                this.descripcionTarea.trim() === "") {
                swal("Campos Vacíos", "Por favor, complete todos los campos", "warning");
                return false;
            }
            return true;
        },
    }
});
</script>

<style scoped>
/* Estilos generales */
body {
    background-color: #c4c4c4;
    /* Cambia el color de fondo aquí */
}

/* Estilos específicos del componente */
.ion-modal {
    font-family: 'Minecraft', sans-serif;
}

.ion-header {
    background-color: #3a5b85;
    /* Cambia el color del encabezado aquí */
}

.ion-header ion-title {
    color: #fff;
    /* Cambia el color del texto del título aquí */
}

.ion-content {
    background-color: #c4c4c4;
    /* Cambia el color del contenido aquí */
}

.ion-item {
    background-color: #fff;
    /* Cambia el color de los elementos de item aquí */
    border-radius: 8px;
    margin-bottom: 12px;
}

.ion-item ion-label {
    color: #3a5b85;
    /* Cambia el color del texto de etiqueta aquí */
}

.ion-item ion-input {
    color: #3a5b85;
    /* Cambia el color del texto de entrada aquí */
}

.ion-footer {
    background-color: #3a5b85;
    /* Cambia el color del pie de página aquí */
}

.ion-footer ion-toolbar {
    justify-content: space-between;
}

.ion-footer ion-button {
    color: #fff;
    /* Cambia el color del texto del botón aquí */
}
</style>