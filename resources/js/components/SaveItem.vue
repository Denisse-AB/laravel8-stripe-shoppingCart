<template>
  <div>
    <button v-bind:id="color" :disabled="isDisabled" @click="submit" class="btn btn-secondary btn-sm float-right m-2" v-text="buttonText"></button>
  </div>
</template>

<script>
export default {
  props: {
    itemId: String,
    checkdb: String,
    lang: String
  },
  data () {
    return {
      status: this.checkdb,
    }
  },
  methods: {
    submit(){
      axios.post(`/saveForLater/${this.itemId}`)
      .then(_response => {
        this.status = ! this.status;
      })
      .catch(errors => {
        if(errors.response.status == 401) {
          window.location = '/login';
        } else {
          window.alert('An error has ocurred!');
        }
      });
    }
  },
  computed: {
    buttonText(){
      if (this.lang == 'en') {
        return (this.status) ? 'Item save in wishlist' :'Save for Later' ;
      } else {
        return (this.status) ? 'Producto en tu lista' :'Guardar en tu lista' ;
      }
    },
    isDisabled(){
      if ((this.status) == true) {
        return true;
      }
    },
    color(){
      if ((this.status) == true) {
        return 'save';
      }
    }
  }
}
</script>

<style scoped>
#save{
  background-color: #158463;
}
</style>
