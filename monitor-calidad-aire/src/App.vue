<template>
  <div id="app">
    <!-- Botón de cierre de sesión -->
    <button class="logout-button" @click="cerrarSesion">Cerrar Sesión</button>

    <h1>Monitor de Calidad del Aire</h1>

    <!-- Sensores de gas -->
    <div class="card sensor" v-for="(sensor, index) in sensors" :key="index">
      <h2>Sensor de Gas {{ index + 1 }}</h2>
      <p>Valor: <span>{{ sensor.value }}</span></p>
    </div>

    <!-- Salidas: LEDs y Buzzer -->
    <div class="card output" v-for="(output, index) in outputs" :key="'output-' + index">
      <h2>Estado de {{ output.name }}</h2>
      <p :class="output.statusClass">{{ output.statusText }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sensors: [
        { name: 'Gas 1', value: 0 },
        { name: 'Gas 2', value: 0 }
      ],
      outputs: [
        { name: 'LED 1', statusText: 'Apagado', statusClass: 'status-off' },
        { name: 'LED 2', statusText: 'Apagado', statusClass: 'status-off' },
        { name: 'Buzzer', statusText: 'Silenciado', statusClass: 'status-silent' }
      ]
    };
  },
  methods: {
    actualizarSensores() {
      // Simulación de valores de sensores
      this.sensors[0].value = Math.floor(Math.random() * 1000);
      this.sensors[1].value = Math.floor(Math.random() * 1000);

      // Lógica para cambiar el estado de los LEDs y el Buzzer
      const threshold = 470;
      if (this.sensors[0].value >= threshold || this.sensors[1].value >= threshold) {
        // Si el valor es mayor o igual a 470, LED rojo, buzzer activo y alerta
        this.outputs[0].statusText = 'Rojo (Alerta)';
        this.outputs[0].statusClass = 'status-off';
        this.outputs[1].statusText = 'Rojo (Alerta)';
        this.outputs[1].statusClass = 'status-off';
        this.outputs[2].statusText = 'Activo';
        this.outputs[2].statusClass = 'status-active';
        alert('¡ALERTA! Los niveles de gas han superado el límite seguro.');
      } else {
        // Si el valor es menor a 470, LED verde, buzzer apagado y todo en orden
        this.outputs[0].statusText = 'Verde (Todo en orden)';
        this.outputs[0].statusClass = 'status-on';
        this.outputs[1].statusText = 'Verde (Todo en orden)';
        this.outputs[1].statusClass = 'status-on';
        this.outputs[2].statusText = 'Silenciado';
        this.outputs[2].statusClass = 'status-silent';
        alert('Todo está en orden. Los niveles de gas son seguros.');
      }
    },
    cerrarSesion() {
      alert('Cerrando sesión...');
      // Aquí puedes agregar la lógica para redireccionar a la página de inicio de sesión
    }
  },
  created() {
    // Ejecutar la actualización cada segundo
    setInterval(this.actualizarSensores, 1000);
  }
};
</script>

<style scoped>
/* Diseño general de la página */
body {
  font-family: Arial, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
  padding: 20px;
  background-color: #e0f7fa;
  position: relative;
}

/* Título y botón de cierre de sesión */
h1 {
  color: #333;
  font-size: 28px;
  margin-bottom: 20px;
  text-align: center;
}

.logout-button {
  position: absolute;
  top: 20px;
  right: 20px;
  padding: 10px 20px;
  background-color: #ff5252;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  font-weight: bold;
}

.logout-button:hover {
  background-color: #ff1744;
}

/* Diseño de tarjetas para sensores y salidas */
.card {
  width: 100%;
  max-width: 500px;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  padding: 20px;
  margin: 10px 0;
  text-align: center;
}

.card h2 {
  margin-top: 0;
  font-size: 20px;
  color: #555;
}

.sensor p,
.output p {
  font-size: 18px;
  margin: 10px 0;
  color: #333;
}

/* Estilo de texto para los valores en tiempo real */
#gasSensor1,
#gasSensor2 {
  font-weight: bold;
  color: #009688;
}

/* Estilo de estado de LED y Buzzer */
#statusLed1,
#statusLed2,
#statusBuzzer {
  font-weight: bold;
  font-size: 18px;
}

/* Colores para los diferentes estados */
.status-on {
  color: green;
}

.status-off {
  color: red;
}

.status-active {
  color: orange;
}

.status-silent {
  color: gray;
}
</style>
