<script context="module" lang="ts">
	type ListItem = {
		name: string;
	};

	type List = ListItem & {
		children: Item[];
		expanded?: boolean;
	};

	export type Item = List | ListItem;

	type Tree = Item[];
</script>

<script lang="ts">
	export let prefix: string = '';
	export let selected: { path: string; item: Item };
	export let tree: Tree;

	const isList = (item: Item): item is List => {
		return Array.isArray((item as List).children);
	};

	const path = (item: Item) => [prefix, item.name].join('/');
</script>

<ul>
	{#each tree as item (item.name)}
		{@const expanded = isList(item) && item.expanded}
		{@const list = isList(item)}
		{@const tree = isList(item) && item.children}
		<li>
			<button
				class:expanded
				class:list
				class:selected={selected?.item === item}
				on:click={() => {
					if (isList(item)) {
						item.expanded = !item.expanded;
					}

					selected = { path: path(item), item };
				}}
			>
				{item.name}
			</button>
			{#if list && expanded}
				<svelte:self {tree} prefix={path(item)} bind:selected />
			{/if}
		</li>
	{/each}
</ul>

<style lang="postcss">
	button {
		border: 0;
		background-color: transparent;
		background: url('/document.svg') left bottom no-repeat;
		height: 20px;
		padding-left: 24px;
		vertical-align: middle;
		cursor: pointer;
	}

	button.list {
		background-image: url('/chevron-right.svg');
	}

	button.expanded {
		background-image: url('/chevron-down.svg');
	}

	button.selected {
		background-color: lightgray;
		border-radius: 0.25em;
	}

	ul {
		list-style-type: none;
		padding-left: 20px;
	}
</style>
