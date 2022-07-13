<script>
  import { createEventDispatcher } from 'svelte';

  export let review;

  // experiment only just incase multiple actions I will add in the later part
  let reviewActions = [ 'Approve', 'Reject', 'Delete' ];

  const dispatch = createEventDispatcher();

  const handleReviewAction = (action, reviewId) => {
    dispatch('handle-review', { action, reviewId });
  }

  $: getReviewStatusClass = () => {
    let result = '';

    if (review.isUpdated) {
      result = review.approved ? 'approved' : 'rejected';
    }

    return result;
  }

  $: getReviewStatusDisabled = (action) => {
    return review.approved && action === 'approve';
  }
</script>

<div class="{getReviewStatusClass()} review-item">
  <strong>{review.score}</strong>
  <blockquote>{review.comments}</blockquote>
  <div class="actions">
    {#each reviewActions as action}
      <button class={action} disabled={getReviewStatusDisabled(action.toLowerCase())} on:click={handleReviewAction(action.toLowerCase(), review.id)}>{action}</button>
    {/each}
  </div>
</div>

<style>
  .review-item{
    display: block;
    margin: 10px auto;
    border: 1px solid #eee;
    background-color: lightgray;
    border-radius: 4px;
    padding: 5px;
  }

  .review-item.approved {
    background-color: lightblue;
  }

  .review-item.rejected {
    background-color: rgb(224, 119, 119);
  }

  .review-item strong{
    border: 1px solid blue;
    background-color: blue;
    color: #fff;
    border-radius: 50px;
    display: inline-block;
    padding: 5px 10px;
    vertical-align: top;
  }

  .review-item .actions{
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .review-item .actions button{
    border: none;
    padding: 5px 15px;
    color: #fff;
    background-color: grey;
    border-radius: 5%;
    margin-right: 5px;
    cursor: pointer;
  }

  .review-item .actions button:disabled {
    color: #999;
  }

  .review-item .actions button:not(:disabled):active {
    background-color: #ddd;
  }

  .review-item .actions button:focus {
    border-color: #666;
  }
</style>