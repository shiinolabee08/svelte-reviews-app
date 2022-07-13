<script>
  import { v4 as uuidv4 } from 'uuid';
	import Reviews from './Reviews.svelte'
	import ReviewStats from './ReviewStats.svelte';
	import ReviewForm from './ReviewForm.svelte';

	let reviews = [];
	let maxReviewScore = 10;
	let minReviewScore = 1;

	for(let i=0; i <= 4; i++) {
		reviews.push({
			id: uuidv4(),
			score: Math.floor(Math.random() * (maxReviewScore - minReviewScore) + minReviewScore),
			approved: false,
			isUpdated: false,
			comments: 'Nulla quis lorem ut libero malesuada feugiat. Donec rutrum congue leo eget malesuada. Vestibulum ac diam sit amet quam vehicula elementum sed sit amet dui.',
		});
	}

	const handleReviewAction = (e) => {

		const { action, reviewId } = e.detail;
		if (null !== reviewId) {
			if (action !== 'delete') {
				const isApproved = action === 'approve' ? true : false;

				reviews = reviews.map((item) => {

					if (item.id === reviewId) {
						item.isUpdated = true;
						item.approved = isApproved;
					}

					return item;
				});
			} else {
				reviews = reviews.filter((item) => item.id !== reviewId);
			}
		}
	}

	const handleAddReviewAction = (e) => {
		const newReview = e.detail;
		reviews = [...reviews, newReview];
	}

</script>

<main class="container">
	<ReviewForm {maxReviewScore} {minReviewScore} on:add-new-review={handleAddReviewAction}/>
	<ReviewStats {reviews}/>
	<Reviews {reviews} on:handle-review={handleReviewAction}/>
</main>

<style>
	.container{
		margin: 10px auto;
		max-width: 768px;
		background-color: '#f4f4f4';
		padding: 0px 20px;
	}
</style>