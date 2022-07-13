<script>
  import { v4 as uuidv4 } from 'uuid';
  import  { createEventDispatcher } from 'svelte';
  export let maxReviewScore;
  export let minReviewScore;

  let reviewScores = [...Array(maxReviewScore)].map((_, index) => index + minReviewScore);

  let minCommentLength = 10;

  let initialState = () => ({
    score: minReviewScore,
    comments: '',
  });

  let newReview = initialState();

  $: message = newReview.comments.trim().length <= minCommentLength ? `You must enter atleast ${minCommentLength} characters.` : '';
  $: btnSubmitDisabled = newReview.comments.trim().length <= minCommentLength;

  const dispatch = createEventDispatcher();

  const handleSubmit = () => {
    if (newReview.comments.trim().length >= minCommentLength) {
      newReview = Object.assign(newReview, {
        id: uuidv4(),
        approved: false,
			  isUpdated: false,
      });

      dispatch('add-new-review', newReview);
    }

    clearFormFields();
  }

  const clearFormFields = () => {
    newReview = initialState();
  }
</script>

<form class="review-form" on:submit|preventDefault={handleSubmit}>
  <h1>Tell us how are on our service that makes you come back.</h1>
  <select bind:value={newReview.score}>
    {#each reviewScores as score}
      <option value={score}>{score}</option>
    {/each}
  </select>
  <input type="text" name="comments" bind:value={newReview.comments}/>
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