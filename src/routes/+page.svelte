<script lang="ts">
	import TreeView, { type Item } from '$lib/TreeView.svelte';

	let tree = [
		{ name: 'README.md' },
		{
			name: 'src',
			expanded: true,
			children: [
				{ name: 'app.js' },
				{ name: 'index.js' },
				{ name: 'handlers', children: [{ name: 'get.js' }, { name: 'index.js' }] }
			]
		},
		{
			name: 'tests',
			children: [{ name: 'index.spec.js' }]
		}
	];

	let selected: { path: string; item: Item };
</script>

<h1>SWE Coding Challenge - TreeView</h1>

Selected item:<code>{selected && selected.path}</code>

<TreeView {tree} bind:selected />

<style lang="postcss">
	code {
		background-color: lightgray;
		border-radius: 0.25em;
		padding: 0.25em 0.5em;
		margin: 0.5em;
	}
</style>
