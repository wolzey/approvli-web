<template>
  <div class="container mx-auto px-4">
    <div class="screenshot"></div>
    <div class="form-container">
      <form>
        <input
          id="comments"
          class="form-comments"
          type="text"
          name="comments"
        />
        <input type="submit" value="approved" class="approve" />
        <input type="submit" value="disapproved" class="disapprove" />
      </form>
    </div>
  </div>
</template>

<script>
export default {
  asyncData({ route: { params } }) {
    console.log({ params })
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
</style>
