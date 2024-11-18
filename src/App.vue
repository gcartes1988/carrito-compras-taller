<template>
  <div class="container mt-5">
    <div class="row">
     
      <!-- Columna de productos -->
      <div class="col-md-6">
        <h2>Productos disponibles</h2>
        <hr />
    
        <ul class="list-group">
        <li v-for="producto in productos" :key="producto.id" class="list-group-item">
          <!-- Contenedor principal: imagen e información -->
          <div class="d-flex align-items-center">
            <!-- Imagen del producto -->
            <img :src="producto.imagen" alt="Imagen del producto" class="producto-imagen me-3"/>
            <!-- Información del producto -->
            <div class="flex-grow-1">
              <span>{{ producto.nombre }}</span> - <span class="fw-bold">Precio: ${{ producto.precio }}</span>
            </div>
          </div>
          <!-- Botón para agregar al carrito -->
          <div class="mt-3">
            <button @click="addToCart(producto)" class="btn btn-purple">
              <i class="bi bi-cart-fill"></i> Agregar al carrito
            </button>
          </div>
        </li>
      </ul>

      </div>

      <!-- Columna del carrito -->
      <div class="col-md-6">
        <h2>Productos en el carrito</h2>
        <hr />
        <ul class="list-group">
          <li v-for="item in carrito" :key="item.product.id" class="list-group-item">
            <!-- Contenedor principal: imagen e información -->
            <div class="d-flex align-items-center">
              <!-- Imagen del producto -->
              <img :src="item.product.imagen" alt="Imagen del producto" class="producto-imagen me-3"/>
              <!-- Información del producto -->
              <div class="flex-grow-1">
                <span>{{ item.product.nombre }}</span><br />
                <span class="fw-bold">Cantidad: {{ item.quantity }}</span>
              </div>
            </div>
            <!-- Botón para remover del carrito -->
            <div class="mt-3">
              <button @click="removeFromCart(item)" class="btn btn-purple">
                <i class="bi bi-trash-fill"></i> Remover del carrito
              </button>
            </div>
          </li>
        </ul>

        
        <!-- Total -->
        <p class="text-left fw-bold fs-4">Total a pagar: ${{ total }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      productos: [
        { id: 1, nombre: "Audífono", precio: 30000, stock: 3, imagen: require('@/assets/audifonos.png') },
        { id: 2, nombre: "Mouse", precio: 20000, stock: 5, imagen: require('@/assets/mouse.jpg') },
        { id: 3, nombre: "Teclado", precio: 15000, stock: 10, imagen: require('@/assets/teclado.jpg') },
        { id: 4, nombre: "Gabinete", precio: 35000, stock: 4, imagen: require('@/assets/gabinete.jpg') },
        { id: 5, nombre: "Pantalla", precio: 175000, stock: 3, imagen: require('@/assets/pantalla.png') },
        { id: 6, nombre: "Silla", precio: 150000, stock: 2, imagen: require('@/assets/silla.jpg') },
      ],
      carrito: [],
    };
  },
  computed: {
    total() {
      return this.carrito.reduce(
        (total, item) => total + item.product.precio * item.quantity,
        0
      );
    },
  },
  methods: {
    addToCart(producto) {
      const cartItem = this.carrito.find(
        (item) => item.product.id === producto.id
      );
      if (cartItem) {
        if (cartItem.quantity < producto.stock) {
          cartItem.quantity++;
        } else {
          alert("¡No puedes agregar más de las unidades disponibles!");
        }
      } else {
        this.carrito.push({ product: producto, quantity: 1 });
      }
    },
    removeFromCart(item) {
      const index = this.carrito.indexOf(item);
      if (index !== -1) {
        if (item.quantity > 1) {
          item.quantity--;
        } else {
          this.carrito.splice(index, 1);
        }
      }
    },
  },
};
</script>
