<template>
  <h1>Logging in...</h1>
</template>

<script>
export default {
  mounted() {
    const urlParams = new URLSearchParams(window.location.search);
    const code = urlParams.get('code');
    const state = urlParams.get('state');

    if (code && state) {
      if (state === window.localStorage.getItem('state')) {
        let params = {
          grant_type: 'authorization_code',
          client_id: 1,
          redirect_uri: 'http://localhost:3000/auth',
          code_verifier: window.localStorage.getItem('verifier'),
          code
        }

        this.$axios.$post('http://localhost:8000/oauth/token', params)
          .then(resp => {
            window.opener.postMessage(resp);
            localStorage.removeItem('state');
            localStorage.removeItem('verifier');
            window.close();
          })
          .catch(e => {
            console.dir(e);
          });
      }
    }
  },
}
</script>
