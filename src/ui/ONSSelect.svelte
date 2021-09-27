<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let options;
	export let selected;
	export let search = false;
	export let placeholder = 'Select an option';
	
	let expanded = false;
	let filter = '';
	let name;
	let el;
	let input;
	
	$: regex = filter ? new RegExp(filter, 'i') : null;
	$: filtered = regex ? options.filter(option => regex.test(option.name)) : options;
	$: (selected || selected == null) && setName();

	function setName() {
		if (selected) {
			name = options.find(d => d.code == selected).name;
		} else {
			name = null;
		}
	}
	
	function toggle(ev) {
		ev.stopPropagation();
		filter = '';
		expanded = !expanded;
	}
	
	$: if (input && expanded) {
		input.focus();
	}
	
	function unSelect(ev) {
		dispatch('select', {
			code: null
		});
		ev.stopPropagation();
		selected = null;
		name = null;
	}

	function submit(ev) {
		if (ev.keyCode === 13 && filtered[0]) {
			ev.preventDefault();
			select(filtered[0]);
		}
	}
	
	function select(option) {
		dispatch('select', {
			code: option.code
		});
		selected = option.code;
		expanded = false;
	}
	
	$: document.onclick = function(e) {
		if(e.target !== el){
			expanded = false;
		}
	};
</script>


<div id="select" class:active={expanded}>
	{#if selected}
	<a id="toggle" class="selected" on:click={toggle}>
		<span>{name}</span>
		<span class="button close" on:click={unSelect}>&nbsp;</span>
	</a>
	{:else}
	<a id="toggle" on:click={toggle}>
		<span>{placeholder ? placeholder : 'Select one'}</span>
		{#if search}
		<span class="button search">&nbsp;</span>
		{:else}
		<span class="button down">&nbsp;</span>
		{/if}
	</a>
	{/if}
	{#if expanded}
	<div id="dropdown" bind:this={el}>
		{#if search == true}
		<input type="text" placeholder="" bind:value={filter} autocomplete="false" bind:this={input} on:keyup={submit} />
		{/if}
		<ul>
			{#if filtered[0]}
			{#each filtered as option}
			<li on:click="{() => select(option)}">{option.name}</li>
			{/each}
			{:else}
			<li>No results</li>
			{/if}
		</ul>
	</div>
	{/if}
	<div class="ons-grid ons-grid--gutterless">
		<div class="ons-grid__col ons-col-8@m">
			<div id="country-of-birth-container" class="ons-js-autosuggest   ons-autosuggest-input" data-instructions="Use up and down keys to navigate suggestions once you&#39;ve typed more than two characters. Use the enter key to select a suggestion. Touch device users, explore by touch or with swipe gestures." data-aria-you-have-selected="You have selected" data-aria-min-chars="Enter 3 or more characters for suggestions." data-aria-one-result="There is one suggestion available." data-aria-n-results="There are 10 suggestions available." data-aria-limited-results="Results have been limited to 10 suggestions. Type more characters to improve your search" data-more-results="Continue entering to improve suggestions" data-results-title="Suggestions" data-no-results="No suggestions found. You can enter your own answer" data-type-more="Continue entering to get suggestions" data-autosuggest-data="https://gist.githubusercontent.com/rmccar/c123023fa6bd1b137d7f960c3ffa1fed/raw/4dede1d6e757cf0bb836228600676c62ceb4f86c/country-of-birth.json">
				<div class="ons-field">
					<label class="ons-label  ons-label--with-description " for="country-of-birth" id="country-of-birth-label">Current name of country
					</label>
					<span id="country-of-birth-label-description-hint" class="ons-label__description  ons-input--with-description">
          Enter your own answer or select from suggestions
        </span>
					<input type="text" id="country-of-birth" class="ons-input ons-input--text ons-input-type__input ons-js-autosuggest-input " autocomplete="off" aria-describedby="country-of-birth-label-description-hint" />
				</div>
				<div class="ons-autosuggest-input__results ons-js-autosuggest-results">
					<header class="ons-autosuggest-input__results-title ons-u-fs-s">Suggestions</header>
					<ul class="ons-autosuggest-input__listbox ons-js-autosuggest-listbox" role="listbox" id="country-of-birth-listbox" aria-labelledby="" tabindex="-1"></ul>
				</div>
				<div class="ons-autosuggest-input__instructions ons-u-vh ons-js-autosuggest-instructions" id="country-of-birth-instructions" tabindex="-1">Use up and down keys to navigate suggestions once you&#39;ve typed more than two characters. Use the enter key to select a suggestion. Touch device users, explore by touch or with swipe gestures.</div>
				<div class="ons-autosuggest-input__status ons-u-vh ons-js-autosuggest-aria-status" aria-live="assertive" role="status" tabindex="-1"></div>
			</div>
		</div>
	</div>
</div>

