<template>
    <div class="container">
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
  
  <style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: sans-serif;
  }

  body {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    background: linear-gradient(#32b767, #18c07d);
  }

  .container {
    border: solid 1px rgba(255, 255, 255, 0.2);
  }

  table {
    color: #fff;
    font-size: 14px;
    table-layout: fixed;
    border-collapse: collapse;
  }

  thead {
    background: rgba(243, 140, 210, 0.4);
  }

  th {
    padding: 20px 15px;
    font-weight: 700;
    text-transform: uppercase;
  }

  td {
    padding: 15px;
    border-bottom: solid 1px rgba(255, 255, 255, 0.2);
  }

  tbody tr {
    cursor: pointer;
  }

  tbody :hover {
    background: rgba(243, 103, 199, 0.4);
  }

  </style>