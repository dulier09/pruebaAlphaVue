<template>
    <div>
      <h1>Gestión de archivos</h1>
      <h2>Seleccionar para procesar y luego mostrar en tabla</h2>
      <p>Selecciona el archivo a gestionar</p>
      <input type="file" @change="leerArchivo2" />
      <h4>Contenido del archivo:</h4>
      <div v-html="tablares"></div>
      <hr/>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tablares: ''
      };
    },
    methods: {
      crearTabla(data) {
        const todasFilas = data.split(/\r?\n|\r/);
        let tabla = '<table>';
        for (let fila = 0; fila < todasFilas.length; fila++) {
          if (fila === 0) {
            tabla += '<thead>';
            tabla += '<tr>';
          } else {
            tabla += '<tr>';
          }
          const celdasFila = todasFilas[fila].split('#');
          for (let rowCell = 0; rowCell < celdasFila.length; rowCell++) {
            if (fila === 0) {
              tabla += '<th>';
              tabla += celdasFila[rowCell];
              tabla += '</th>';
            } else {
              tabla += '<td>';
              tabla += celdasFila[rowCell];
              tabla += '</td>';
            }
          }
          if (fila === 0) {
            tabla += '</tr>';
            tabla += '</thead>';
            tabla += '<tbody>';
          } else {
            tabla += '</tr>';
          }
        } 
        tabla += '</tbody>';
        tabla += '</table>';
        this.tablares = tabla;
      },
  
      leerArchivo2(event) {
        const file = event.target.files[0];
        const reader = new FileReader();
  
        reader.onload = (e) => {
          // Cuando el archivo se terminó de cargar
          this.crearTabla(e.target.result);
        };
  
        // Leemos el contenido del archivo seleccionado
        reader.readAsText(file);
      }
    }
  };
  </script>
  
  <style scoped>
  </style>