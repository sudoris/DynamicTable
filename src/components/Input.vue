<template>
  <input :type="inputType" readonly> 
</template>

<script>
export default {
  name: 'Input',
  props: ['type', 'toggleEdit'],
  computed: {
    inputType() {
      if (this.type === 'phone') {
        return 'tel'
      } 
      else if (this.type === 'email') {
        return 'email'
      }
      else {
        return 'text'
      }
    }
  }
}
</script>

<style>

</style>
