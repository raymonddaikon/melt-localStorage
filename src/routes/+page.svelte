<script lang="ts">
	import { browser } from '$app/environment';
	import { createTabs, melt } from '@melt-ui/svelte';
	const triggers = [
		{ id: 'tab-1', title: 'Account' },
		{ id: 'tab-2', title: 'Password' },
		{ id: 'tab-3', title: 'Settings' }
	];
	const {
		elements: { root, list, content, trigger },
		states: { value }
	} = createTabs({
		loop: true,
		defaultValue: 'tab-1',
		// value: valueStore,
		orientation: 'vertical',
		autoSet: true, // If no value is given, the value state will be set to the first tab item provided on mount.
		onValueChange: (value) => {
			if (browser) {
				console.log(`before = ${window.localStorage.getItem('tab')}`);
				window.localStorage.setItem('tab', value.next);
				console.log(`after = ${window.localStorage.getItem('tab')}`);
			}
			return value.next;
		}
	});
</script>

<div use:melt={$root}>
	<div use:melt={$list} aria-label="Manage your account">
		{#each triggers as triggerItem}
			<button use:melt={$trigger(triggerItem.id)} class="trigger">
				{triggerItem.title}
			</button>
		{/each}
	</div>
	<div use:melt={$content('tab-1')} class="grow bg-white p-5">
		<p class="mb-5 leading-normal text-neutral-900">
			Make changes to your account here. Click save when you're done.
		</p>
		<fieldset class="mb-4 flex w-full flex-col justify-start">
			<label class="mb-2.5 block text-sm leading-none text-neutral-900" for="name"> Name </label>
			<input id="name" value="Thomas G. Lopes" />
		</fieldset>

		<div class="mt-5 flex justify-end">
			<button class="save">Save changes</button>
		</div>
	</div>
	<div use:melt={$content('tab-2')} class="grow bg-white p-5">
		<p class="mb-5 leading-normal text-neutral-900">
			Change your password here. Click save when you're done.
		</p>
		<fieldset class="mb-4 flex w-full flex-col justify-start">
			<label class="mb-2.5 block text-sm leading-none text-neutral-900" for="new">
				New password
			</label>
			<input id="new" type="password" />
		</fieldset>
		<div class="mt-5 flex justify-end">
			<button class="save">Save changes</button>
		</div>
	</div>
	<div use:melt={$content('tab-3')} class="grow bg-white p-5">
		<p class="mb-5 leading-normal text-neutral-900">
			Change your settings here. Click save when you're done.
		</p>

		<fieldset class="mb-4 flex w-full flex-col justify-start">
			<label class="mb-2.5 block text-sm leading-none text-neutral-900" for="new">
				New email
			</label>
			<input id="new" type="password" />
		</fieldset>
		<div class="mt-5 flex justify-end">
			<button class="save">Save changes</button>
		</div>
	</div>
</div>
