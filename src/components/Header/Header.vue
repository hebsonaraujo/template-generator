<template>
    <header>
        <h1> {{ msg }} </h1>
    </header>
</template>

<script>
export default {
  name: 'HeaderPreview',
  props: {
    msg: String
  }
}
</script>