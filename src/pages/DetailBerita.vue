<template>
  <q-page>
    <q-card>
      <q-card-section>
        <h2>{{ berita.title }}</h2>
        <p>{{ berita.description }}</p>
        <p>{{ berita.content }}</p>
        <a :href="berita.url" target="_blank">Baca lebih lanjut</a>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
export default {
  props: {
    berita: Object // Menerima berita sebagai prop
  }
}
</script>
