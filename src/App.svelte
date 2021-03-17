<script>
	import ContactCard from './ContactCard.svelte';

	let formState = 'Empty';
	let contacts = [];
	
	let name = '';
	let description = '';
	let jobTitle = '';
	let profileImage = ''

	// $: = Reactive variable
	$: uppercaseName = name.toUpperCase();
	$: console.log(name);

	function autoPopulateData() {
		profileImage = 'https://www.jondjones.com/images/background/jondjones_search.png';
		name = 'Jon';
		description='A person';
		jobTitle = 'A legend'
	}

	function addContactCard() {

		if (
			name.trim().length == 0 ||
			description.trim().length == 0 ||
			jobTitle.trim().length == 0 ||
			profileImage.trim().length == 0
		) {
			formState = 'invalid'
			console.log('formState', formState);
		}  else {
			// Always update arrays using =
			// = sign triggeres to svelte to check whats changed
			// this is why push/pop will do nothing
			// create a new array, rather than update an old one
			contacts = [
				...contacts,
				{
					id: Math.random(),
					name,
					description, 
					jobTitle,
					profileImage
				}]
			clearForm()
		}
	}

	function clearForm()
	{
		name = '';
		description = '';
		jobTitle = '';
		profileImage = '';

		formState = true;
	}

	function nameInput(e) {
		name = e.target.value;
		console.log(name);
	}

	function deleteFirstContactCard() {
		contacts = contacts.slice(1);
	}

	function deleteLastContactCard() {
		contacts = contacts.slice(0, -1)
	}
</script>

<style>
	.container {
		display: flex;
	}

	.button-container{
		width: 10%;;
		margin-left: 3rem;
	}

	button {
			margin-top: 1rem;
		}
	.error-message {
		color: #cc0505;
	}

	#form {
    	width: 40%;
    	max-width: 100%;
  	}

	.no-contacts {
		padding: 0.5rem 0;
		display: block;
		width: 100%;
		border: none;
		border-bottom: 2px solid #ccc;
		border-radius: 3px 3px 0 0;
		background: white;
		padding: 1rem;
		transition: border-color 0.1s ease-out;
		font-weight: bold;
	}
</style>

{#if uppercaseName}
	<h1>
		Hello {uppercaseName}
	</h1>
{:else}
	<h1>
		Welcome To ProfileBook
	</h1>
{/if}

<div class="container">
	<form id="form">
		<div class="form-control">
			<!-- Two-way Binding -->
			<label for="userName">User Name</label>
			<input type="text" 
				value={name} 
				on:input={nameInput}  
				placeholder="Username"
				id="userName" />
		</div>
		<div class="form-control">
			<!-- Shortcut Two-way binding syntax -->
			<label for="jobTitle">Job Title</label>
			<input 
				type="text" 
				bind:value={jobTitle} 
				placeholder="Job Title"
				id="jobTitle" />
		</div>
		<div class="form-control">
			<label for="profileImage">Profile Image</label>
			<input 
				type="text" 
				bind:value={profileImage} 
				placeholder="Profile Image"
				id="profileImage" />
		</div>
		<div class="form-control">
			<label for="desc">Description</label>
			<textarea  rows="3" bind:value={description} placeholder="Description"/>
		</div>

		<!-- Modifiers once/passive/capture/stop propegration/prevent default -->
		<button on:click|preventDefault="{addContactCard}" type="submit">Add Card</button>

	</form>
	
	<div class="button-container">
		<button on:click="{autoPopulateData}">Auto Populate Card</button>
		<button on:click|once="{deleteFirstContactCard}">Delete First Card</button>
		<button on:click="{deleteLastContactCard}">Delete Last Card</button>
	</div>
</div>

{#if formState === 'invalid'}
	<div class="error-message">
		All form fields need to be filled in
	</div>
{/if}

{#each contacts as contact, index (contact.id)}
	<h2>#{index}</h2>
	<ContactCard 
		userName={contact.name} 
		jobTitle={contact.jobTitle} 
		description={contact.description} 
		profileImage={contact.profileImage} />
{:else}
	<div class="no-contacts">
		No Contacts Added Yet
	</div>
{/each}