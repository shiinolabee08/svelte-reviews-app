<script>
  import ReviewItem from './ReviewItem.svelte';

  export let reviews = [];

</script>

<div class="reviews">
  {#each reviews as review (review.id)}
    <ReviewItem {review} on:handle-review/>
  {/each}
</div>

<style>
  .reviews {
    position: relative;
  }
</style>