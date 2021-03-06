<template>
  <div>
  <q-btn-group>
    <q-btn
      v-for="(button, index) in buttons"
      :key="index"
      :color="button.color"
      :label="button.label"
      :text-color="button.textColor"
      @click="setActive(button, index)"
    ></q-btn>
  </q-btn-group>
  <q-btn
    style="margin-left: 10px"
    color="primary"
    label="초기화"
    @click="onReset"
  />
  </div>
</template>

<script>
const initFilter = () => {
  return [
    {
      label: '생존인물만',
      isActive: false,
      color: 'white',
      textColor: 'black',
    },
    {
      label: '여자',
      isActive: false,
      color: 'white',
      textColor: 'black',
    },
    {
      label: 'tvSeries 없음',
      isActive: false,
      color: 'white',
      textColor: 'black',
    },
  ]
}
export default {
  name: "SearchFilter",
  data () {
    return {
      color: 'white',
      buttons: initFilter(),
    }
  },
  methods: {
    setActive(button, index) {
      if(button.isActive) {
        this.buttons[index].color = 'white'
        this.buttons[index].textColor = 'black'
        this.buttons[index].isActive = false
      } else {
        this.buttons[index].color = 'primary'
        this.buttons[index].textColor = 'white'
        this.buttons[index].isActive = true
      }

      this.$emit('FILTER_UPDATE')
    },
    onReset() {
      this.buttons = initFilter()
    }
  }
}
</script>

<style scoped>

</style>