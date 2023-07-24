<template>
    <ion-modal :is-open="mostrarModalEdicion" @dismiss="cerrarModal" style="max-height: auto;">
        <ion-header>
            <ion-toolbar style="background-color: #20262E;">
                <ion-title style="color: #fff;">Editar la Tarea {{ tareaSeleccionada.nombreTarea }}</ion-title>
                <ion-buttons slot="end">
                    <ion-button @click="cerrarModal">
                        <ion-icon name="close"></ion-icon>
                    </ion-button>
                </ion-buttons>
            </ion-toolbar>
        </ion-header>
        <ion-content style="background-color: #20262E">
            <ion-card>
                <ion-card-content>
                    <ion-item>
                        <ion-label position="floating" style="color: #FFFFFF; font-size: 18px;">Id de la Tarea</ion-label>
                        <ion-input v-model="tareaSeleccionada.id" type="text" :disabled="true" style="font-size: 18px;"></ion-input>
                    </ion-item>
                    <ion-item>
                        <ion-label position="floating" style="color: #FFFFFF font-size: 18px;">Nombre de la Tarea</ion-label>
                        <ion-input v-model="tareaSeleccionada.nombreTarea" type="text" style="font-size: 18px;"></ion-input>
                    </ion-item>
                    <ion-item>
                        <ion-label position="floating" style="color: #FFFFFF; font-size: 18px;">Descripcion de la Tarea</ion-label>
                        <ion-input v-model="tareaSeleccionada.descripcionTarea" type="text" style="font-size: 18px;"></ion-input>
                    </ion-item>
                    <ion-item>
                        <ion-label position="floating" style="color: #FFFFFF; font-size: 18px;">Fecha de Creacion</ion-label>
                        <ion-input v-model="tareaSeleccionada.fechaCreacion" type="text" :disabled="true" style="font-size: 18px;"></ion-input>
                    </ion-item>
                </ion-card-content>
            </ion-card>
            <ion-footer>
                <ion-toolbar>
                    <ion-buttons slot="start">
                        <ion-button color="white" @click="cerrarModal"
                            style="background-color: #263A29; border-radius: 10px; font-size: 18px;">
                            <i class="fas fa-times fa-xl" style="margin-right: 10px;"></i> Cancelar
                        </ion-button>
                    </ion-buttons>
                    <ion-buttons slot="end">
                        <ion-button color="white" @click="guardarCambios"
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
        tareaSeleccionada: {
            type: Object,
            required: true
        },
        editarTareaModal: {
            type: Boolean,
            required: true
        }
    },

    data() {
        return {
            editarTarea: false,
            mostrarModalEdicion: this.editarTareaModal,
        };
    },
    methods: {
        abrirModal() {
            this.$emit('update:mostrarModalEdicion', true);
            // this.editarProductoModal = true;
        },
        validarCampos() {
            if (
                this.tareaSeleccionada.nombreTarea.trim() === "" ||
                this.tareaSeleccionada.descripcionTarea.trim() === ""
            ) {
                swal("Campos Vacíos", "Por favor, complete todos los campos", "warning");
                return false;
            }
            return true;
        },

        cerrarModal() {
            this.$emit('dismiss');
            this.$emit('update:mostrarModalEdicion', false);
            this.mostrarModalEdicion = false;
            //this.editarProductoModal = false;
            this.tareaSeleccionada.nombreTarea = '';
            this.tareaSeleccionada.descripcionTarea = '';
        },

        async guardarCambios() {
            try {
                if (!this.validarCampos()) {
                    return;
                } else {

                    const response = await fetch(`https://localhost:44305/api/Tareas/${this.tareaSeleccionada.id}`, {
                        method: "PUT",
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify(this.tareaSeleccionada),
                    });
                    if (response.ok) {
                        //this.products = this.products.filter((producto) => producto.id !== id);
                        //const sound = new Audio('../sounds/xp.mp3');
                        //sound.play();
                        swal("Actualización Exitosa", "La Tarea Se Actualizó Correctamente", "success");
                        setTimeout(() => {
                            location.reload();
                        }, 2000);
                        // Actualizar la lista de productos después de actualizar
                        this.$emit('update:editarTareaModal', false);
                        this.$emit('dismiss');

                    } else {
                        swal("Error", "No Se Puede Actualizar la Tarea", "error");
                        console.error('Error al actualizar el producto', response);
                    }
                }
            } catch (error) {
                swal("Error", error, "error");
                console.error('Error al actualizar la Tarea', error);
            }
        }
    }
});
</script>
<style scoped>
/* Estilos generales */
body {
    background-color: #c4c4c4;
    /* Cambia el color de fondo aquí */
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