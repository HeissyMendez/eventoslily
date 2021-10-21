<template>
  <div>
    {{ Titulo }}
  </div>
</template>

<script>
export default {
  name: "ColumTable",
  props: {
    Hora: String,
    Sesiones: Array,
  },
  data() {
    return {
      Titulo: "",
    };
  },
  mounted() {
    this.Validarsesion();
  },
  methods: {
    Validarsesion() {
      this.Sesiones.forEach((sesion) => {
        let Horasesion = sesion.start.substr(11, 2);

        if (Horasesion == this.Hora) {
          this.Titulo = sesion.title;
        }
      });
    }
  }
}
</script>