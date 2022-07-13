<script>
  import  { createEventDispatcher } from 'svelte';
  export let maxReviewScore;
  export let minReviewScore;

  let reviewScores = [...Array(maxReviewScore)].map((_, index) => index + minReviewScore);

  let minCommentLength = 10;

  let initialState = () => ({
    score: minReviewScore,
    comments: null,
  });

  let newReview = initialState();

  let btnSubmitDisabled = true;

  let message = null;

  const handleInput = () => {
    if (newReview.comments !== null && newReview.comments.trim().length <= minCommentLength) {
      message = `You must enter atleast ${minCommentLength} characters.`;
      btnSubmitDisabled = true;
    } else {
      message = null;
      btnSubmitDisabled = false;
    }
  }

  const dispatch = createEventDispatcher();

  const handleSubmit = (e) => {
    e.preventDefault();

    dispatch('add-new-review', newReview);

    clearFormFields();
  }

  const clearFormFields = () => {
    newReview = initialState();
  }
</script>

<form class="review-form" on:submit={handleSubmit}>
  <h1>Tell us how are on our service that makes you come back.</h1>
  <select bind:value={newReview.score}>
    {#each reviewScores as score}
      <option value={score}>{score}</option>
    {/each}
  </select>
  <input type="text" name="comments" on:input={handleInput} bind:value={newReview.comments}/>
  <button type="submit" disabled={btnSubmitDisabled}>Submit</button>

  {#if message}
    <div class="message">{message}</div>
  {/if}
</form>

<style>
  .message{
    color: purple;
  }
</style>