<template>
  <div class="form-control">
    <label for="type">Тип блока</label>

    <select :id="selectId" :value="select" @change="$emit('update', $event.target.value)">
      <option value="title">Заголовок</option>
      <option value="subtitle">Подзаголовок</option>
      <option value="avatar">Аватар</option>
      <option value="text">Текст</option>
    </select>
  </div>
</template>

<script>
export default {
  emits: ['update'],
  props: {
    select: {
      type: String
    }
  },
  data () {
    return {
      selectId: 'select-' + Math.floor(Math.random())
    }
  }
}
</script>
