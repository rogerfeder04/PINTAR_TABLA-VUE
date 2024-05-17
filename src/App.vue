<template>
  <div class="container" style="text-align: center;">
    <label for="nombre">Nombre del producto a insertar:</label>
    <input type="text" id="nombre" v-model="nombre" placeholder="Nombre" @input="validarCampo">
    <br><br>
    <label for="proveedor">Nombre del Proveedor:</label>
    <input type="text" id="proveedor" v-model="proveedor" placeholder="Proveedor" @input="validarCampo">
    <br><br>
    <label for="precio">Digite el Precio al publico:</label>
    <input type="number" id="precio" v-model.number="precio" placeholder="Precio" min="0" @input="validarCampo">
    <br><br>
    <label for="costo">Digite el Costo al mayor:</label>
    <input type="number" id="costo" v-model.number="costo" placeholder="Costo" min="0" @input="validarCampo">
    <br><br>

    <label for="cantidad">Digite la Cantidad de compra:</label>
    <input type="number" id="cantidad" v-model.number="cantidad" placeholder="Cantidad" min="0" @input="validarCampo">
    <br><br>
  
    <button @click="agregarProducto" :disabled="!formularioValido">Agregar Producto</button>
    <br><br>
    <!-- <br><br> -->



    <table v-if="productos.length > 0">
      <!-- Tabla -->
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Proveedor</th>
          <th>Precio</th>
          <th>Costo</th>
          <th>Cantidad</th>
          <th>Ganancia</th>
          <th>Opciones</th>
        </tr>
      </thead>
      <tbody>
        <!-- Bucle para cada producto en la lista -->
        <tr v-for="(item, i) in productos" :key="i">
          <td>{{ item.nombre }}</td>
          <td>{{ item.proveedor }}</td>
          <td>{{ item.precio }}</td>
          <td>{{ item.costo  }}</td>
          <td>{{ item.cantidad }}</td>
          <td>{{ item.ganancia  }}</td>
          <td>
            <!--  aumenta o disminuye la cantidad del producto -->
            <button @click="cambiarCantidad(item, 1)">+</button>
            <button @click="cambiarCantidad(item, -1)" :disabled="item.cantidad <= 0">-</button>
          </td>
        </tr>
      </tbody>
    </table>
    
    <div v-if="productos.length > 0">
       <!-- totales de costo, precio y ganancia del inventario -->
      <p>Costo total del inventario: {{ costoTotal }}</p>
      <p>Precio total ventas del inventario  : {{ precioTotal  }}</p>
      <p>Ganancia total ventas: {{ gananciaTotal  }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const nombre = ref('');
const proveedor = ref('');
const precio = ref(0);
const costo = ref(0);
const cantidad = ref(0);
const productos = ref([]);

const agregarProducto = () => {
  if (formularioValido) {
    const nuevoProducto = {
      nombre: nombre.value,
      proveedor: proveedor.value,
      precio: precio.value,
      costo: costo.value,
      cantidad: cantidad.value,
      ganancia: precio.value - costo.value,
    };
    productos.value.push(nuevoProducto);
    resetForm();
  }
};

const cambiarCantidad = (producto, delta) => {
  const index = productos.value.indexOf(producto);
  if (index !== -1 && producto.cantidad + delta >= 0) {
    productos.value[index].cantidad += delta;
  }
};

const costoTotal = computed(() => {
  return productos.value.reduce((acc, p) => acc + p.costo * p.cantidad, 0);
});

const precioTotal = computed(() => {
  return productos.value.reduce((acc, p) => acc + p.precio * p.cantidad, 0);
});

const gananciaTotal = computed(() => {
  return precioTotal.value - costoTotal.value;
});

const validarCampo = () => {
  formularioValido.value = nombre.value.trim() !== '' && precio.value > 0 && costo.value > 0 && cantidad.value > 0 && proveedor.value.trim() !== '';
};

const formularioValido = ref(false);

const resetForm = () => {
  nombre.value = '';
  proveedor.value = '';
  precio.value = 0;
  costo.value = 0;
  cantidad.value = 0;
  formularioValido.value = false;
};
</script>









<style>
body {
  font-family: Arial, sans-serif;
  /* background-color: #203a6d; */
  margin: 0;
  padding: 0;
}

.container {
  max-width: 1000px;
  margin: 1px auto;
  padding: 15px;
  border-radius: 10px;
    background-color: #e5dada;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* Estilos para etiquetas y campos de entrada */
label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

input[type="text"],
input[type="number"],
button {
  padding: 10px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  /* font-size: 16px; */
  width: auto;
  box-sizing: border-box;
}

input[type="number"],
button {
  width: auto;
}

button {
  background-color: #4CAF50;
  color: rgb(232 232 232);
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:disabled {
  background-color: #4b4040;
  cursor: not-allowed;
}

button:hover {
  background-color: #45a049;
}

/* Estilos para la tabla */
/* table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  text-align: left;
}

th {
  background-color: #070707;
}

/* Estilos para los totales */
.total {
  margin-top: 20px;
}

.total label {
  font-weight: bold;
}

.total p {
  margin: 5px 0;
  font-size: 18px;
} 

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  border-right: 1px solid #ddd; /* Agregar líneas verticales */
  text-align: left;
}

th {
  background-color: #070707;
  color: white; /* Color de texto para encabezados */
}

/* Estilos para los totales */
.total {
  margin-top: 20px;
}

.total label {
  font-weight: bold;
}

.total p {
  margin: 5px 0;
  font-size: 18px;
}
</style>