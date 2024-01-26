<template>
  <div id="app">
    <h1>Sistema de Notas de Alumnos</h1>
    <form @submit.prevent="agregarAlumno" class="form">
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input v-model="nuevoAlumno.nombre" type="text" required>
      </div>
      <div class="form-group">
        <label for="apellido">Apellido:</label>
        <input v-model="nuevoAlumno.apellido" type="text" required>
      </div>
      <div class="form-group">
        <label for="nota">Nota:</label>
        <input v-model="nuevoAlumno.nota" type="number" min="0" max="20" step="0.1" required>
      </div>
      <button type="submit" class="btn-agregar">Agregar Alumno</button>
    </form>

    <table class="tabla">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Apellido</th>
          <th>Nota</th>
          <th>Resultado</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(alumno, index) in alumnos" :key="index" class="alumno">
          <td>{{ alumno.nombre }}</td>
          <td>{{ alumno.apellido }}</td>
          <td>
            <span v-if="!alumno.editando">{{ alumno.nota }}</span>
            <input v-if="alumno.editando" v-model="alumno.nota" type="number" min="0" max="20" step="0.1">
          </td>
          <td>{{ alumno.resultado }}</td>
          <td>
            <button @click="eliminarAlumno(index)" class="btn-eliminar">Eliminar</button>
            <button @click="toggleEdicion(index)" class="btn-editar">{{ alumno.editando ? 'Guardar' : 'Editar' }}</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nuevoAlumno: { nombre: "", apellido: "", nota: "" },
      alumnos: [],
    };
  },
  methods: {
    agregarAlumno() {
      if (this.nuevoAlumno.nota >= 0 && this.nuevoAlumno.nota <= 20) {
        this.nuevoAlumno.resultado = this.nuevoAlumno.nota >= 10 ? "Aprobado" : "Desaprobado";
        this.alumnos.push({ ...this.nuevoAlumno, editando: false });
        this.nuevoAlumno = { nombre: "", apellido: "", nota: "" };
      } else {
        alert("La nota debe estar entre 0 y 20.");
      }
    },
    eliminarAlumno(index) {
      if (confirm("¿Estás seguro de que deseas eliminar este alumno?")) {
        this.alumnos.splice(index, 1);
      }
    },
    toggleEdicion(index) {
      const alumno = this.alumnos[index];
      if (alumno.editando) {
        // Guardar la nota editada
        alumno.resultado = alumno.nota >= 10 ? "Aprobado" : "Desaprobado";
      }
      alumno.editando = !alumno.editando;
    },
  },
};
</script>

<style>
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  color: #333;
  margin-top: 60px;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 10px;
  width: 100%;
}

label {
  display: block;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
}

.btn-agregar {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
}

.tabla {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.alumno .btn-eliminar {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 8px 12px;
  margin-right: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.alumno .btn-eliminar:hover {
  background-color: #d32f2f;
}

.alumno .btn-editar {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.alumno .btn-editar:hover {
  background-color: #2980b9;
}
</style>
