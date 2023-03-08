<template>
  <div class="container" @click.self="resetSelected">
    <v-add-sensor
      :max_id="maxSensorsId"
      :selectedSensor="selectedSensor"
      @add="add"
      @click="resetSelected"
    />
    <template v-if="sensors.length">
      <hr />
      <section class="sensors-wrapper" @click.self="resetSelected">
        <div v-for="sensor in sensors" :key="sensor.sensor_id">
          <v-sensor
            :data="sensor"
            :highlighted="sensor.sensor_id === selectedSensor?.sensor_id"
            @editSensor="editSensor"
            @deleteSensor="deleteSensor"
          />
        </div>
      </section>
      <hr />
    </template>
    <button class="btn btn_active" type="button" @click="uploadData">
      Загрузить датчики
    </button>
  </div>
</template>

<script>
import sensors from './assets/events.json'
import VAddSensor from './components/V-AddSensor.vue'
import VSensor from './components/V-Sensor.vue'
export default {
  name: 'App',

  components: { VSensor, VAddSensor },
  data() {
    return {
      sensors: [],
      selectedSensor: null
    }
  },
  // Загрузка данных из localstorage
  created() {
    if (localStorage.getItem('sensors')) {
      const storageSensors = JSON.parse(localStorage.getItem('sensors'))
      this.sensors = storageSensors
    }
  },
  methods: {
    add(sensorToAdd) {
      this.sensors = [
        ...this.sensors.filter(s => s.sensor_id !== sensorToAdd.sensor_id),
        sensorToAdd
      ].sort((a, b) => a.sensor_id - b.sensor_id)
      this.selectedSensor = null
    },
    editSensor(sensorToEdit) {
      this.selectedSensor = sensorToEdit
    },
    deleteSensor(sensorToRemove) {
      this.sensors = this.sensors.filter(s => s !== sensorToRemove)
    },
    uploadData() {
      this.sensors = sensors
    },
    resetSelected() {
      this.selectedSensor = null
    }
  },
  computed: {
    // Вычисление максимального ID для добавлени нового датчика
    maxSensorsId() {
      return this.sensors.length > 0
        ? Math.max(...this.sensors.map(sensor => sensor.sensor_id)) + 1
        : 0
    }
  },
  watch: {
    // Отслеживает изменения массива sensors и сохраняет данные в localstorage
    sensors() {
      localStorage.setItem('sensors', JSON.stringify(this.sensors))
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans:wght@300;400;500;700;900&display=swap');
#app {
  font-family: 'Noto Sans', sans-serif;
  color: #fff;
}

* {
  padding: 0px;
  margin: 0px;
}
body {
  background-color: #181920;
}

hr {
  width: 100%;
  margin: 15px 0px;
  height: 1px;
  border: none;
  border-top: 2px solid #dae685;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #242630;
  max-width: 1050px;
  margin: auto;
  padding: 0px 10px;
}

.sensors-wrapper {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1.5rem;
}

.btn {
  background-color: #ffa555;
  border-radius: 12px;
  color: #000;
  cursor: pointer;
  font-weight: bold;
  padding: 15px 15px;
  text-align: center;
  transition: 0.3s ease;
  width: 100%;
  box-sizing: border-box;
  border: 0;
  font-family: 'Noto Sans', sans-serif;
  max-height: 60px;
  max-width: 120px;
  min-height: 35px;
  min-width: 0px;
  overflow: hidden;
  padding: 5px;
  font-size: 14px;
  margin-bottom: 15px;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.btn:hover {
  color: #fff;
  background-color: #181920;
}

.btn_active {
  background-color: #dae685;
}

.btn-delete {
  background-color: #d83a00;
}
</style>
