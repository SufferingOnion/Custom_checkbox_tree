<template id="vue-checkbox">
  <div class=''>
    <label class="custom-checkbox">
      <input
          type='checkbox'
          :checked='item.checked'
          :indeterminate.prop="item.indeterminate"
          @change='onChange($event.target.checked)'>
      <span>{{ item.label }}</span>
    </label>
    <template v-if='item.children'>
      <vue-checkbox
          v-for='(item, index) in item.children'
          ref='children'
          :item='item'
          :key="index"
          @change='onChildChange'>
      </vue-checkbox>
    </template>
  </div>
</template>

<script>
export default {
  name: 'vue-checkbox',
  template: '#vue-checkbox',
  props: ['item'],
  methods: {
    onChange(checked) {
      this.item.checked = checked;
      if (this.item.children) {
        this.item.indeterminate = false;
      }
      this.updateParent()
      this.updateChildren(checked)
    },
    onChildChange() {
      this.item.checked = this.$refs.children.every(child => child.item.checked);
      if (this.item.checked){
        this.item.indeterminate = false;
      } else {
        this.item.indeterminate = this.$refs.children.some(child => child.item.checked || child.item.indeterminate);
      }
      this.updateParent()
    },
    updateParent() {
      this.$emit('change');
    },
    updateChildren(checked) {
      if (this.item.children) {
        this.$refs.children.forEach(child => {
          child.item.checked = checked;
          child.item.indeterminate = false;
          child.updateChildren(checked)
        })
      }
    }
  }
}
</script>

<style scoped lang="scss">
/* для элемента input c type="checkbox" */
.custom-checkbox>input {
  position: absolute;
  z-index: -1;
  opacity: 0;
}

/* для элемента label, связанного с .custom-checkbox */
.custom-checkbox>span {
  display: inline-flex;
  align-items: center;
  user-select: none;
}

/* создание в label псевдоэлемента before со следующими стилями */
.custom-checkbox>span::before {
  content: '';
  display: inline-block;
  width: 1em;
  height: 1em;
  flex-shrink: 0;
  flex-grow: 0;
  border: 1px solid #adb5bd;
  border-radius: 0.25em;
  margin-right: 0.5em;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 50% 50%;
}

/* стили при наведении курсора на checkbox */
.custom-checkbox>input:not(:disabled):not(:checked)+span:hover::before {
  border-color: #b3d7ff;
}

/* стили для активного чекбокса (при нажатии на него) */
.custom-checkbox>input:not(:disabled):active+span::before {
  background-color: #b3d7ff;
  border-color: #b3d7ff;
}

/* стили для чекбокса, находящегося в фокусе */
.custom-checkbox>input:focus+span::before {
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

/* стили для чекбокса, находящегося в фокусе и не находящегося в состоянии checked */
.custom-checkbox>input:focus:not(:checked)+span::before {
  border-color: #80bdff;
}

/* стили для чекбокса, находящегося в состоянии checked */
.custom-checkbox>input:checked+span::before {
  border-color: #0b76ef;
  background-color: #0b76ef;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 8'%3e%3cpath fill='%23fff' d='M6.564.75l-3.59 3.612-1.538-1.55L0 4.26 2.974 7.25 8 2.193z'/%3e%3c/svg%3e");
}

/* стили для чекбокса, находящегося в состоянии disabled */
.custom-checkbox>input:disabled+span::before {
  background-color: #e9ecef;
}

/* стили для чекбокса, находящегося в состоянии indeterminate */
.custom-checkbox>input:indeterminate+span::before {
  background-image:url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 4 4'%3e%3cpath stroke='%23007bff' d='M0 2h4'/%3e%3c/svg%3e")!important;
  border-radius: 3px;

  border-color: #007bff;

}
</style>