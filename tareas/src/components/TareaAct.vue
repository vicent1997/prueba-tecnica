<template>
  <div class="container mt-5">
    <div class="row mb-5 d-flex">
      <label style="color: white"> Filtro </label>
      <select
        class="form-control"
        v-model="filtroPrioridad"
        v-on:change="ordenenarTareas()"
      >
        <option value="1">todas</option>
        <option value="2">Alta</option>
        <option value="3">Media</option>
        <option value="4">Baja</option>
      </select>
    </div>
    <label class="text-white"> Nueva tarea </label>
    <br />
    <label class="text-white"> Prioridad </label>

    <div class="row d-flex">
      <div class="col-lg-4">
        <select id="prioridad" class="form-control" v-model="nuevaPrioridad">
          <option value="2" selected>Alta</option>
          <option value="3">Media</option>
          <option value="4">Baja</option>
        </select>
        <button class="btn btn-secondary" @click="agregarTarea">
          Agregar Tarea
        </button>
      </div>
      <div class="col-lg-8">
        <input
          type="text"
          class="form-control mr-2"
          placeholder="Nombre de la tarea"
          v-model="nombreTarea"
        />
        <input
          type="text"
          class="form-control mr-2"
          placeholder="Descripción de la tarea"
          v-model="descripcionTarea"
        />
      </div>
    </div>
    <div class="row">
      <div class="col-lg-6 offset-lg-3 mt-3">
        <div class="card p-1" v-if="tareas.length === 0">
          <h6>No hay tareas para mostrar</h6>
        </div>
        <div v-else>
          <ul
            class="list-group"
            v-for="(tarea, index) in tareas"
            v-bind:key="index"
          >
            <li class="list-group-item d-flex justify-content-between">
              <span class="cursor" v-on:click="actualizarTarea(tarea, index)">
                <i
                  v-bind:class="[
                    tarea.estado
                      ? 'fa-solid fa-circle-check'
                      : 'fa-regular fa-circle',
                  ]"
                ></i>
              </span>
              <div>
                <h5>{{ tarea.nombre }}</h5>
                <input
                  type="text"
                  class="form-control"
                  v-model="tarea.descripcion"
                />
              </div>
              <span
                class="cursor text-danger"
                v-on:click="eliminarTarea(index)"
              >
                <i class="fa-solid fa-trash"></i>
              </span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tareas: [],
      tareasAux: [],
      nombreTarea: "",
      descripcionTarea: "",
      nuevaPrioridad: "2",
      filtroPrioridad: "1",
    };
  },
  methods: {
    agregarTarea() {
      const tarea = {
        nombre: this.nombreTarea,
        descripcion: this.descripcionTarea,
        estado: false,
        prioridad: parseInt(this.nuevaPrioridad),
      };
      this.tareas.push(tarea);
      this.nombreTarea = "";
      this.descripcionTarea = "";
      this.tareasAux = this.tareas;
    },
    actualizarTarea(tarea, index) {
      this.tareas[index].estado = !tarea.estado;
    },
    eliminarTarea(index) {
      if (this.filtroPrioridad !== "1") {
        const tareaAEliminar = this.tareas[index].nombre;
        const indexEnAux = this.tareasAux.findIndex(
          (tarea) => tarea.nombre === tareaAEliminar
        );
        if (indexEnAux !== -1) {
          this.tareasAux.splice(indexEnAux, 1);
        }
      }
      this.tareas.splice(index, 1);
    },
    ordenenarTareas() {
      this.tareas = this.tareasAux;
      if (this.filtroPrioridad !== "1") {
        this.tareas = this.tareas.filter(
          (filteredException) =>
            filteredException.prioridad === parseInt(this.filtroPrioridad)
        );
      } else {
        this.tareas = this.tareasAux;
      }
    },
  },
};
</script>

<style scoped>
.cursor {
  cursor: pointer;
}
input {
  text-align: center;
}
</style>
