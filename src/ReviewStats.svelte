<script>
  export let reviews;

  $: count = reviews.length;
  $: averageTotal = (reviews.reduce((prev, {score}) => prev + score, 0) / reviews.length).toFixed(1);
</script>

<div class="review-stats">
  <h3>{count} Review(s)</h3>
  <h3>Average of {averageTotal}</h3>
</div>

<style>
  .review-stats {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .review-stats > h3:first-child{
    flex: 5;
  }

  .review-stats > h3:last-child {
    flex: 1;
  }
</style>