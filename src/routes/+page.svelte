<script lang="ts">
	let avatar, fileinput;

	const onFileSelected = (e) => {
		let image = e.target.files[0];
		let reader = new FileReader();
		reader.readAsDataURL(image);
		reader.onload = (e) => {
			avatar = e.target?.result;
		};
	};
	import { enhance } from '$app/forms';
	export let form;
</script>

<div id="app">
	<h1>Upload Image</h1>
	{#if avatar}
		<div class="avatar" style="--src:url({avatar})" />
	{/if}
	<div>
		<form
			class="grid gap-6 w-full"
			action="?/upload"
			method="POST"
			enctype="multipart/form-data"
			use:enhance={() => {
				return async ({ update }) => {
					update({ reset: true });
				};
			}}
		>
			<div>
				<div class="space-y-1 mb-4">
					<h2 class="text-xl font-semibold">Upload a file</h2>
					<p class="text-sm text-gray-500">Accepted formats: .png, .jpg, .gif, .mp4</p>
				</div>
				<div class="mt-1 flex rounded-md shadow-sm">
					<input
						style=""
						type="file"
						accept=".jpg, .jpeg, .png"
						on:change={(e) => onFileSelected(e)}
						bind:this={fileinput}
						name="file"
					/>
				</div>
			</div>

			<button>
				<p class="text-sm">Confirm upload</p>
			</button>
		</form>
	</div>
</div>

<style>
	#app {
		display: flex;
		align-items: center;
		justify-content: center;
		flex-flow: column;
	}

	.avatar {
		width: 200px;
		height: 200px;
		background-image: var(--src);
		background-position: center;
		background-size: cover;
		background-repeat: no-repeat;
	}
</style>
