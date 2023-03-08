// V-AddSensor - форма для добавление новых датчиков

<template>
  <section class="add-sensor">
    <form class="form" @submit.prevent="onSubmit">
      <h3 class="title">Добавить / изменить датчик:</h3>
      <label for="sensor_id" @click.stop>Номер датчика</label>
      <input
        id="sensor_id"
        type="number"
        :value="selectedSensor ? sensor.sensor_id : max_id"
        disabled="disabled"
        @click.stop.prevent
      />
      <label for="name" @click.stop>Название</label>
      <input
        id="name"
        type="text"
        v-model="sensor.name"
        placeholder="BB-8"
        maxlength="20"
        @click.stop.prevent
      />
      <label for="temperature" @click.stop>Температура</label>
      <input
        id="temperature"
        type="number"
        v-model="sensor.temperature"
        placeholder="℃"
        min="-273"
        @click.stop.prevent
      />
      <label for="humidity" @click.stop>Влажность</label>
      <input
        id="humidity"
        type="number"
        v-model="sensor.humidity"
        placeholder="%"
        min="0"
        max="100"
        @click.stop.prevent
      />
      <button class="btn add-btn" @click.stop type="submit">Сохранить</button>
      <em>*Чтобы изменить датчик, кликните на его карточку</em>
    </form>
  </section>
</template>

<script>
export default {
  name: 'AddSensor',
  props: {
    max_id: {
      type: Number,
      required: true
    },
    selectedSensor: {
      type: [Object, null],
      required: true
    }
  },
  data() {
    return {
      sensor: {
        name: null,
        temperature: null,
        humidity: null
      }
    }
  },
  methods: {
    onSubmit() {
      let newSensor = {
        sensor_id:
          this.selectedSensor === null ? this.max_id : this.sensor.sensor_id,
        name: this.sensor.name === null ? 'N/A' : this.sensor.name,
        temperature: this.sensor.temperature,
        humidity: this.sensor.humidity
      }
      this.sensor.name = null
      this.sensor.temperature = null
      this.sensor.humidity = null
      this.$emit('add', newSensor)
    }
  },
  watch: {
    selectedSensor() {
      if (this.selectedSensor === null)
        this.sensor = {
          name: null,
          temperature: null,
          humidity: null
        }
      else {
        this.sensor = { ...this.selectedSensor }
      }
    }
  }
}
</script>

<style scoped>
.add-sensor {
  margin: 30px 0 15px 0;
}

.form {
  display: flex;
  align-items: center;
  flex-direction: column;
  padding: 10px;
  border: 3px solid #dae685;
  border-radius: 12px;
}

.title {
  margin: 15px 0;
}

.add-btn {
  min-width: 200px;
  margin: 20px 0 15px 0;
}

label {
  font-weight: 500;
}

input {
  width: 200px;
  padding: 6px 20px;
  margin: 8px 0;
  box-sizing: border-box;
  border: 3px solid #ccc;
  -webkit-transition: 0.5s;
  transition: 0.5s;
  outline: none;
  border-radius: 12px;
  font-family: 'Noto Sans', sans-serif;
  font-size: 15px;
  font-weight: 700;
}

input[type='text']:focus {
  border: 3px solid #555;
}

input:disabled {
  background-color: #cccccc94;
}

@media screen and (min-width: 800px) {
  .form {
    min-width: 700px;
  }
}
</style>
