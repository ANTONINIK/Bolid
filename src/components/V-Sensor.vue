// V-Sensor - карточка датчика

<template>
  <div class="sensor" @click.stop="handleEdit" :class="{ sensor_active: highlighted }">
    <div class="sensor__about">
      <dl class="about__list">
        <dt>Номер датчика</dt>
        <dd>{{ data.sensor_id }}</dd>
        <template v-if="data.name">
          <dt>Название</dt>
          <dd>{{ data.name }}</dd>
        </template>
        <template v-if="showDescription">
          <template v-if="data.temperature">
            <dt>Температура</dt>
            <dd>{{ data.temperature }} ℃</dd>
          </template>
          <template v-if="data.humidity">
            <dt>Влажность</dt>
            <dd>{{ data.humidity }} %</dd>
          </template>
          <div class="list__extend">
            <h4>Доп. информация:</h4>
            <p>Ошибок не обнаружено</p>
          </div>
        </template>
      </dl>
    </div>
    <div class="sensor__buttons">
      <button
        class="btn"
        type="button"
        :class="{ btn_active: showDescription }"
        @click.stop="this.showDescription = !this.showDescription"
      >
        Подробнее
      </button>
      <button class="btn btn-delete" type="button" @click.stop="handleDelete">
        Удалить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VSensor',
  data() {
    return {
      showDescription: false
    }
  },
  props: {
    data: {
      type: Object,
      required: true
    },
    highlighted: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    // Обработчики кнопок
    handleDelete() {
      this.$emit('deleteSensor', this.data)
    },
    handleEdit() {
      this.$emit('editSensor', this.data)
    }
  }
}
</script>

<style scoped>
.sensor {
  min-width: 270px;
  padding: 0 30px;
  border-radius: 12px;
  border: 2px solid #808080;
  flex-grow: 1;
  transition: 0.3s all;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.sensor__about {
  padding: 10px;
}

.sensor:hover {
  border: 2px solid #d83a00;
}

.sensor_active {
  border: 2px solid #d83a00;
}

.about__list {
  margin: 0 0 0px 0;
  overflow: hidden;
}

.about__list dt {
  font-weight: 600;
  width: 200px;
  float: left;
  clear: both;
  margin: 0 0px 0 0;
  padding: 5px 0;
  border-top: 1px solid #ccc;
}

.about__list dd {
  margin: 0 0 0 200px;
  padding: 5px 0;
  border-top: 1px solid #ccc;
}

.about__list dt:first-of-type,
.about__list dd:first-of-type {
  border-top: none;
}

.sensor__buttons {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.list__extend {
  margin: 10px;
  text-align: center;
}
</style>
