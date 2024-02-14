<template>
  <div>
    <div class="alertas">
      <div class="modal" v-if="modal">
        <div class="modal-content alerta">
          <span class="closeicon" @click="cerrar()"><i class="fa fa-times"></i></span>
          <div class="modal-body">
            <span class="icon"><i class="fa fa-exclamation-circle"></i></span>
            <div class="right-items">
              <div class="titulo-alert">
                <h1>¡Alerta!</h1>
              </div>
              <div class="texto-alert">
                <h3 id="alert" v-if="error != ''">{{ error }}</h3>
              </div>
              <button @click="cerrar()" class="okbtn">Aceptar</button>
            </div>
          </div>
        </div>
      </div>
      <div class="modal2" v-if="modal2">
        <div class="modal-content exito">
          <span class="closeicon2" @click="cerrar()"><i class="fa fa-times"></i></span>
          <div class="modal-body">
            <span class="icon"><i class="fa fa-thumbs-up"></i></span>
            <div class="right-items">
              <div class="titulo-alert">
                <h1>¡Exito!</h1>
              </div>
              <div class="texto-alert">
                <h3 id="alert2" v-if="alert != ''">{{ alert }}</h3>
              </div>
              <button class="okbtn2" @click="cerrar()">Aceptar</button>
            </div>
          </div>
        </div>
      </div>

    </div>
    <div class="contenedor">
      <h1 class="titulo">Agenda</h1>
      <div class="contenedores">
        <div class="cont_">
          <label for="">Asignar Tarea:</label>
          <input id="actividad" type="text" placeholder="Ingrese Tarea que va a realizar" v-model="actividad">
        </div>
        <div class="cont_fecha cont_">
          <label for="">Fecha:</label>
          <input id="" type="date" v-model="fecha">
        </div>
        <div class="botones">
          <select name="" id="options" class="seleccion" v-model="tipo">
            <option disabled selected hidden value="">Seleccione Nivel de Prioridad</option>
            <option value="Alta">Alta</option>
            <option value="Mediana">Mediana</option>
            <option value="Baja">Baja</option>
          </select>
          <button class="btn_add" @click="validar()">Agregar</button>
          <button class="btn" @click="ordenar()">Ordenar</button>
        </div>
      </div>
      <table>
        <thead>
          <tr>
            <th>Actividad</th>
            <th>Prioridad</th>
            <th>Fecha</th>
            <th>Opciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, i) in datos" :key="i" :style="{ backgroundColor: item.tipo === 'Alta' ? '#E18181' : (item.tipo === 'Mediana' ? '#C8CC95' : (item.tipo === 'Baja' ? '#6ED2A9' : '')) }">
            <td>{{ item.actividad }}</td>
            <td>{{ item.fecha }}</td>
            <td>{{ item.tipo }}</td>
            <td>
              <button class="boton" @click="borrar()">
                <i class="fa fa-trash"></i>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue"
let datos = ref([]);
let actividad = ref("");
let fecha = ref("");
let tipo = ref("");
let modal = ref(false);
let modal2 = ref(false);
let error = ref("");
let alert = ref("");

function validar() {
  if (actividad.value == "") {
    error.value = "Por favor digite la actividad"
    modal.value = true;
    setTimeout(() => {
      error.value = ""
      modal.value = false;
    }, 5000);

  } else if (fecha.value == "") {
    error.value = "Por favor digite la fecha"
    modal.value = true;
    setTimeout(() => {
      error.value = ""
      modal.value = false;
    }, 5000);
  } else if (tipo.value == "") {
    error.value = "Por favor seleccione el tipo de prioridad"
    modal.value = true;
    setTimeout(() => {
      error.value = ""
      modal.value = false;
    }, 5000);

  } else {
    alert.value = "Registro exitoso"
    modal2.value = true;
    setTimeout(() => {
      alert.value = ""
      modal2.value = false;
    }, 5000);
    agregar()
  }
}

function agregar() {

  const agregar = {
    actividad: actividad.value,
    fecha: fecha.value,
    tipo: tipo.value
  }
  datos.value.push(agregar);
  console.log(datos.value);
  limpiar();
  console.log(datos.value);
}

function cerrar() {
  modal.value = false;
  modal2.value = false;

}

function limpiar() {
  actividad.value = "";
  fecha.value = "";
  tipo.value = "";
}

function ordenar() {
  datos.value.sort((a, b) => {

    const orden = { 'Alta': 0, 'Mediana': 1, 'Baja': 3 };

    return orden[a.tipo] - orden[b.tipo];
  });

  
}

function borrar(i) {
  datos.value.splice(i,1)
}

</script>



