<template>
  <div>
    <vue-checkbox :item='rootItem'></vue-checkbox>
    <button @click="SendData">
      Отправить родителю
    </button>
  </div>
</template>

<script>
import VueCheckbox from "@/components/VueCheckbox";
export default {
  name: 'vue-form',
  data(){
    return{
      rootItem: {
        label: 'Select All',
        checked: false,
        indeterminate:false,
        children: [{
          label: 'Group 1',
          checked: false,
          indeterminate:false,
          children: [{
            label: 'Subgroup 1',
            checked: false,
            indeterminate:false,
            children: [
              { label: 'Item 1', checked: false },
              { label: 'Item 2', checked: false }
            ]
          }, {
            label: 'Subgroup 2',
            checked: false,
            indeterminate:false,
            children: [
              { label: 'Item 3', checked: false },
              { label: 'Item 4', checked: false }
            ]
          }]
        }, {
          label: 'Group 2',
          checked: false,
          indeterminate:false,
          children: [
            { label: 'Item 5', checked: false },
            { label: 'Item 6', checked: false }
          ]
        }]
      }
    }
  },
  methods: {
    SendData(){
      this.$emit( 'senddata', this.rootItem)
    }
  },
  components: {
    'vue-checkbox': VueCheckbox
  }
}
</script>


