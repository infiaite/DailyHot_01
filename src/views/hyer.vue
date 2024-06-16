<script>
export default {

  mounted() {
    this.goToUrl();
  },
  methods: {
    goToUrl() {
      const url = 'http://www.hyermall.com';
      window.location.href = url;
    }
  }
}



</script>
