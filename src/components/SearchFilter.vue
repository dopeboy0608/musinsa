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
import Events from "@/event/Events";

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
      buttons: initFilter(),
      setId: null,
      delay: 300,
    }
  },
  methods: {
    setActive(button, index) {
      if (this.setId) {
        clearTimeout(this.setId)
        this.setId = null
      }
      if(button.isActive) {
        this.buttons[index].color = 'white'
        this.buttons[index].textColor = 'black'
        this.buttons[index].isActive = false
      } else {
        this.buttons[index].color = 'primary'
        this.buttons[index].textColor = 'white'
        this.buttons[index].isActive = true
      }
      if (index === 0) {
        this.isAlive = this.buttons[index].isActive
      }

      if (index === 1) {
        this.gender = this.buttons[index].isActive
      }

      if (index === 2) {
        this.isTvSeries = this.buttons[index].isActive
      }

      this.setId = setTimeout(() => {
        this.$emit(Events.FILTER_UPDATE, this.createFilterOptions())
      }, this.delay)
    },
    onReset() {
      if (this.setId) {
        clearTimeout(this.setId)
        this.setId = null
      }
      this.buttons = initFilter()
      this.setId = setTimeout(() => {
        this.$emit(Events.FILTER_UPDATE, this.createFilterOptions(), true)
      }, this.delay)
    },
    createFilterOptions() {
      return {
        isAlive: this.buttons[0].isActive,
        isFemale: this.buttons[1].isActive,
        isTvSeries: this.buttons[2].isActive,
      }
    }
  }
}
</script>

<style scoped>

</style>