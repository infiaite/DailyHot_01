<script>
export default {
  mounted() {
    this.redirectToExternalUrl();
  },
  methods: {
    redirectToExternalUrl() {
      const externalUrl = 'http://www.hyermall.com';
      window.location.href = externalUrl;
    }
  }
}
</script>

