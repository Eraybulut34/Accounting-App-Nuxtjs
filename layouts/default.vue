<template>
<div>
    <div>
  <b-navbar toggleable="lg" type="dark" variant="info">

    <b-navbar-toggle class="pr-5" target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav class="mr-5">
      <b-navbar-nav>
        <b-row class="pl-6">
        <b-nav-item to="/">Home</b-nav-item>
        <b-nav-item to="/girdiler">Entries</b-nav-item>
        </b-row>
              </b-navbar-nav>

    </b-collapse>
  </b-navbar>
</div>
    <Nuxt/>
</div>
</template>
<script>
export default {
}
</script>

<style scoped>



</style>