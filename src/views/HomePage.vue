<template>
  <ion-page>
    <ion-header>
      <ion-toolbar style="background-color: #4E6C50;">
        <ion-title><i class="fa-duotone fa-basket-shopping-simple" style="margin-right: 10px;"></i>Lista de
          Tareas</ion-title>
        <ion-buttons slot="end">
          <ion-button style="font-size: 17.5px;" @click="abrirModalNuevo">
            <i class="fas fa-plus fa-lg" style="margin-right: 10px;"></i> Crear Tarea
          </ion-button>
        </ion-buttons>

      </ion-toolbar>
    </ion-header>

    <ion-content style="background-color: #263A29;" ref="content">
      <ion-row class="d-flex align-center justify-center animated-row fade-in">
        <ion-col cols="12" md="6" class="animated-column fade-in">
          <ion-item class="ion-align-items-center">
            <ion-icon slot="start" name="search" style="color: #6f9e4a;"></ion-icon>
            <ion-label position="floating" style="color: #FFFFFF">Buscar por
              ID</ion-label>
            <ion-input v-model="searchById" @input="buscarTareaId"></ion-input>
          </ion-item>
        </ion-col>
      </ion-row>

      <ion-row class="d-flex align-center justify-center">
        <ion-col v-for="tarea in task" :key="tarea.Id" size-md="3">
          <ion-card :id="'card-' + tarea.id" class="hover-scale animated-card fade-in mx-auto align-center"
            max-width="auto"
            style="border: 5px solid #263A29; border-radius: 10px; background-color: #263A29; color: #FF0000"
            elevation="10">
            <ion-img :src="imagen" height="200px" style="border-radius: 100px;" />
            <ion-card-header class="font-weight-bold" style="color: #FFFFFF;">
              <ion-card-title style="font-size: 25px; color: #F2E3DB">{{ tarea.nombreTarea }}</ion-card-title>
            </ion-card-header>
            <hr style="border-color: #146b63;" />
            <ion-card-content>
              <ion-card-subtitle>
                <ion-card-text class="font-weight-bold" style="color: #F2E3DB; font-size: 17.5px">Código:
                  {{ tarea.id
                  }}</ion-card-text> <br /> <br />
                <ion-card-text class="font-weight-bold" style="color: #F2E3DB; font-size: 17.5px;">Descripción: {{
                  tarea.descripcionTarea }}</ion-card-text> <br /> <br />
                <ion-card-text class="font-weight-bold" style="color: #F2E3DB; font-size: 17.5px;">
                  Fecha de Creación:
                  {{
                    new Date(tarea.fechaCreacion).toLocaleString('es-ES', {
                      weekday: 'long',
                      day: 'numeric',
                      month: 'long',
                      year: 'numeric',
                      hour: 'numeric',
                      minute: 'numeric',
                      second: 'numeric'
                    })
                  }}
                </ion-card-text>
                <br /> <br />

              </ion-card-subtitle>
              <ion-row class="ion-align-items-center ion-justify-content-center">
                <ion-col>
                  <ion-button expand="full" color="success" @click="abrirModal(tarea)"
                    style="font-family: 'Minecraft', sans-serif;">
                    <i class="fas fa-edit fa-lg" style="margin-right: 10px;"></i> Editar
                  </ion-button>
                </ion-col>
                <ion-col>
                  <ion-button expand="full" color="danger" @click="confirmarEliminacion(tarea.id)"
                    style="font-family: 'Minecraft', sans-serif;">
                    <i class="fas fa-trash fa-lg" style="margin-right: 10px;"></i> Eliminar
                  </ion-button>
                </ion-col>
              </ion-row>

            </ion-card-content>
          </ion-card>
        </ion-col>
      </ion-row>
      <!-- Componente del formulario de guardar producto -->
      <GuardarTareaModal v-bind:nuevaTarea="nuevaTarea" formTitle="Nueva Tarea" @dismiss="resetModal"
        @actualizarTareas="obtenerTareas" />

      <EditarTareaModal :tareaSeleccionada="tareaSeleccionada" :editarTareaModal="mostrarModalEdicion"
        @dismiss="resetModal" :is-open="mostrarModalEdicion" />

      <ion-footer style="background-color: #41644A;">
        <ion-container>
          <ion-row>
            <ion-col cols="12" md="4" class="text-center text-md-left" style="font-size: 20px">
              <h4 class="white--text">Sobre nosotros</h4>
              <p class="white--text">Somos una API conectada a Redis</p>
            </ion-col>
            <ion-col cols="12" md="4" class="text-center" style="font-size: 20px;">
              <h4 class="white--text">Redes Sociales</h4>
              <ion-button fab small color="indigo" class="mx-2 hover-color"
                style="height: 55px; width: 55px; border: 5px solid #263A29;"
                href="https://www.facebook.com/Lord.Peacock.890/" target="_blank">
                <i class="fab fa-facebook fa-2xl"></i>
              </ion-button>
              <ion-button fab small color="indigo" class="mx-2 hover-color"
                style="height: 55px; width: 55px; border: 5px solid #263A29;"
                href="https://www.instagram.com/iam_lordp890/" target="_blank">
                <i class="fab fa-instagram fa-2xl"></i>
              </ion-button>
              <ion-button fab small color="indigo" class="mx-2 hover-color" href="https://pin.it/610vSg5" target="_blank"
                style="height: 55px; width: 55px; border: 5px solid #263A29;">
                <i class="fab fa-pinterest fa-2xl"></i>
              </ion-button>
              <ion-button fab small color="indigo" class="mx-2 hover-color"
                style="height: 55px; width: 55px; border: 5px solid #263A29;"
                href="https://www.tumblr.com/blog/renardrouge890" target="_blank">
                <i class="fab fa-tumblr fa-2xl"></i>
              </ion-button>
              <ion-button fab small color="indigo" class="mx-2 hover-color"
                style="height: 55px; width: 55px; border: 5px solid #263A29;" href="https://github.com/FerdinandSpier890"
                target="_blank">
                <i class="fab fa-github fa-2xl"></i>
              </ion-button>
            </ion-col>

            <ion-col cols="12" md="4" class="text-center text-md-right" style="font-size: 20px">
              <h4 class="white--text">Contacto</h4>
              <p class="white--text">Correo electrónico: 20300019@uttt.edu.mx</p>
              <p class="white--text">
                Teléfono:
                <a href="https://api.whatsapp.com/send?phone=527732265327&text=Hola,%20quiero%20enviar%20un%20mensaje"
                  target="_blank">+52 (773) 226-5327</a>
              </p>
            </ion-col>
          </ion-row>
        </ion-container>
      </ion-footer>
    </ion-content>
  </ion-page>
</template>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonRow, IonCol, IonCard, IonImg, IonCardHeader, IonCardTitle, IonCardContent, IonButton, IonIcon, IonFab, IonFabButton, IonItem, IonLabel, IonInput, IonAlert } from '@ionic/vue';
import { defineComponent } from 'vue';
import swal from 'sweetalert';
import GuardarTareaModal from "@/components/NuevaTarea.vue";
import EditarTareaModal from "@/components/EditarTarea.vue";

export default defineComponent({
  components: {
    IonPage,
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonRow,
    IonCol,
    IonCard,
    IonImg,
    IonCardHeader,
    IonCardTitle,
    IonCardContent,
    IonButton,
    IonIcon,
    IonFab,
    IonFabButton,
    IonItem,
    IonLabel,
    IonInput,
    IonAlert,
    GuardarTareaModal,
    EditarTareaModal

  },
  data() {
    return {
      nuevaTarea: false,
      task: [],
      imagen: 'https://cdn-icons-png.flaticon.com/512/7820/7820331.png',
      formTitle: '',
      id: '',
      nombreTarea: '',
      descripcionTarea: '',
      fechaCreacion: '',
      searchById: '',
      tareaSeleccionada: {
        id: '',
        nombreTarea: '',
        descripcionTarea: '',
        fechaCreacion: '',
      }, // Asegúrate de tener el objeto del producto seleccionado
      mostrarModalEdicion: false
    };
  },
  created() {
    this.mostrarTareas();

    this.$nextTick(() => {
      const contentElement = this.$refs.content?.$el;
      if (contentElement) {
        contentElement.addEventListener('click', this.resetModalEdicion);
      }
    });
    //this.musicaFondo();
  },
  methods: {
    beforeUnmount() {
      const contentElement = this.$refs.content?.$el;
      if (contentElement) {
        contentElement.removeEventListener('click', this.resetModalEdicion);
      }
    },
    resetModal() {
      this.nuevaTarea = false;
      this.mostrarModalEdicion = false;
      this.formTitle = '';
      this.id = '';
      this.nombreTarea = '';
      this.descripcionTarea = '';
      this.fechaCreacion = '';
      this.nuevaTarea = false;
      this.mostrarModalEdicion = false;
    },
    async mostrarTareas() {
      try {
        const response = await fetch('https://localhost:44305/api/Tareas');
        if (!response.ok) {
          swal("Error", "No Se Puede Obtener las Tareas", "error")
        }
        const data = await response.json();
        //const audio = new Audio('../sounds/xp.mp3');
        //audio.currentTime = 0;
        //audio.play();
        this.task = data;
        //this.$refs.guardarTareaModal.mostrarTareas(); // Llamar al método mostrarProductos() en el componente hijo
      } catch (error) {
        swal("Error", error, "error")
      }
    },
    async eliminarTarea(id) {
      try {
        const response = await fetch(`https://localhost:44305/api/Tareas/${id}`, {
          method: 'DELETE',
        });

        if (!response.ok) {
          swal("Error", "No Se Puede Eliminar la Tarea", "error")
        }
        //const audio = new Audio('../sounds/netherenter.mp3');
        //audio.currentTime = 0;
        //audio.play();
        swal("Tarea Eliminada", "La Tarea Se Elimino Correctamente", "success")
        const card = document.getElementById(`card-${id}`);
        card.classList.add('burn-effect');

        setTimeout(() => {
          this.mostrarTareas();
        }, 5000);
      } catch (error) {
        swal("Error", error, "error")
      }
    },
    confirmarEliminacion(id) {
      swal({
        title: 'Confirmación',
        text: '¿Estás Seguro de que Quieres Eliminar esta Tarea?',
        icon: 'warning',
        buttons: {
          cancel: {
            text: "No",
            value: null,
            visible: true,
            className: "",
            closeModal: true,
          },
          confirm: {
            text: "Si",
            value: true,
            visible: true,
            className: "",
            closeModal: true
          }
        }
      }).then((result) => {
        if (result) {
          this.eliminarTarea(id);
        } else {
          // swal("Error", "No Se Puede Eliminar El Producto", "error")
        }
      });
    },
    actualizarTareas() {
      this.mostrarTareas();
    },
    async buscarTareaId() {
      if (this.searchById === "") {
        this.mostrarTareas();
      } else {
        try {
          const response = await fetch(`https://localhost:44305/api/Tareas/${this.searchById}`);
          if (!response.ok) {
            return;
          }
          const data = await response.json();
          this.task = [data];
        } catch (error) {
          this.mostrarToast('No Se Pudo Buscar El Producto Por ID', 'error');
        }
      }
    },
    abrirModalNuevo() {
      this.nuevaTarea = true;
    },
    abrirModal(tarea) {
      this.mostrarModalEdicion = true;
      this.tareaSeleccionada.id = tarea.id;
      this.tareaSeleccionada.nombreTarea = tarea.nombreTarea;
      this.tareaSeleccionada.descripcionTarea = tarea.descripcionTarea;
      this.tareaSeleccionada.fechaCreacion = tarea.fechaCreacion;
    },
    // Agrega el método de restablecerModal() para restablecer las variables de edición del producto
    restablecerModal() {
      this.mostrarModalEdicion = false;
      this.tareaSeleccionada.id = '';
      this.tareaSeleccionada.nombreTarea = '';
      this.tareaSeleccionada.descripcionTarea = '';
      this.tareaSeleccionada.fechaCreacion = '';
    },
    musicaFondo() {
      const audio = new Audio('../sounds/sweden.mp3');
      audio.play();
    },
    resetModalEdicion() {
      this.mostrarModalEdicion = false;
      this.nuevaTarea = false;
    },
  },
  computed: {
    searchIcon() {
      return this.searchByCategory || this.searchById ? 'close-circle' : 'search';
    },
  },
});
</script>

<style scoped>
.animated-card {
  animation: fade-in 0.5s ease-in-out;
}

@keyframes fade-in {
  0% {
    opacity: 0;
    transform: scale(0.8);
  }

  100% {
    opacity: 1;
    transform: scale(1);
  }
}

.hover-scale {
  transition: transform 0.3s ease-in-out;
}

.hover-scale:hover {
  transform: scale(1.05);
}

hr {
  margin-top: 10px;
  margin-bottom: 10px;
  border: 0;
  border-top: 1px solid #263A29;
}

.hover-elevate {
  transition: transform 0.3s ease-in-out;
}

.hover-elevate:hover {
  transform: translateY(-10px);
}


.animated-row {
  animation: fade-in 0.5s ease-in-out;
}

.animated-column {
  animation: fade-in 0.5s ease-in-out;
  animation-delay: 0.2s;
  /* Agrega un retraso de animación para los elementos de columna */
}

@keyframes fade-in {
  0% {
    opacity: 0;
    transform: translateX(-10px);
  }

  100% {
    opacity: 1;
    transform: translateX(0);
  }
}

.animated-card-destroy {
  animation: destroy-card 0.5s ease-in-out;
  opacity: 0;
  transform: scale(0.8);
}

@keyframes destroy-card {
  0% {
    opacity: 1;
    transform: scale(1);
  }

  100% {
    opacity: 0;
    transform: scale(0.8);
  }
}

.typewriter-effect {
  overflow: hidden;
  /* Oculta el contenido que se va a escribir */
  white-space: nowrap;
  /* Evita que el texto se divida en múltiples líneas */
  animation: typing 3s steps(30) infinite;
}

@keyframes typing {
  from {
    width: 0;
  }

  to {
    width: 100%;
  }
}

.animated-card {
  position: relative;
  transition: box-shadow 0.3s ease-in-out;
}

.animated-card:hover {
  animation: star-glow 1s infinite alternate;
}

@keyframes star-glow {
  0% {
    box-shadow: 0 0 100px 0 #263A29;
  }

  50% {
    box-shadow: 0 0 200px 0 #263A29;
  }

  100% {
    box-shadow: 0 0 100px 0 #263A29;
  }
}

@keyframes enlarge-disappear {
  0% {
    opacity: 1;
    transform: scale(1);
  }

  100% {
    opacity: 0;
    transform: scale(3) rotate(360deg);
  }
}

.animated-card-remove {
  animation: enlarge-disappear 1s ease-in-out;
}

.ion-fab {
  animation: fade-in 0.5s ease-in-out;
}

@keyframes fade-in {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}

@keyframes burn-animation {
  0% {
    opacity: 1;
    transform: scale(1);
    background-color: #263A29;
    color: #7833ae;
  }

  50% {
    opacity: 0.5;
    transform: scale(1.1);
    background-color: #5e1c9e;
    color: #FFFFFF;
  }

  100% {
    opacity: 0;
    transform: scale(1.2);
    background-color: #3f1187;
    color: #FFFFFF;
  }
}

.burn-effect {
  animation: burn-animation 5s linear forwards;
  animation-delay: 0.2s;
  /* Agrega un retraso de animación para que se muestre la animación después de eliminar el producto */
}

.hover-color:hover {
  background-color: #F2E3DB;
  border: 10px solid #F2E3DB;
  border-radius: 10px;
}

.hover-color:hover i {
  color: #41644A;
}

.swal-button {
  background-color: #146b63
}
</style>