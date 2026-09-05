<script lang="ts">
	const email = 'hello@osemudiame.com';

	const links = [
		{ label: 'LinkedIn', href: 'https://www.linkedin.com/in/charles-okojie' },
		{ label: 'GitHub', href: 'https://github.com/charliwize' }
	];

	let time = $state('');
	let copied = $state(false);

	function formatTime() {
		const parts = new Intl.DateTimeFormat('en-GB', {
			hour: '2-digit',
			minute: '2-digit',
			second: '2-digit',
			hour12: false,
			timeZone: 'Europe/Helsinki',
			timeZoneName: 'shortOffset'
		}).formatToParts(new Date());

		const get = (type: Intl.DateTimeFormatPartTypes) =>
			parts.find((part) => part.type === type)?.value ?? '';

		return `${get('hour')}:${get('minute')}:${get('second')} ${get('timeZoneName')}`;
	}

	async function copyEmail() {
		await navigator.clipboard.writeText(email);
		copied = true;
		setTimeout(() => {
			copied = false;
		}, 2000);
	}

	$effect(() => {
		time = formatTime();
		const interval = setInterval(() => {
			time = formatTime();
		}, 1000);

		const onKeyDown = (event: KeyboardEvent) => {
			if (event.key !== 'c' && event.key !== 'C') return;
			if (event.metaKey || event.ctrlKey || event.altKey) return;

			const target = event.target;
			if (target instanceof HTMLInputElement || target instanceof HTMLTextAreaElement) return;

			event.preventDefault();
			void copyEmail();
		};

		window.addEventListener('keydown', onKeyDown);

		return () => {
			clearInterval(interval);
			window.removeEventListener('keydown', onKeyDown);
		};
	});
</script>

<svelte:head>
	<title>Osemudiame Okojie</title>
	<meta
		name="description"
		content="Software engineer at DoorDash and founder of Terraglance. Based in Helsinki, Finland."
	/>
</svelte:head>

<main class="mx-auto min-h-screen max-w-xl px-6 py-16 sm:py-20">
	<header class="mb-10 flex items-center justify-between text-xs text-neutral-400">
		<span>HELSINKI, FI</span>
		<span class="flex items-center gap-1.5 tabular-nums">
			<svg
				class="size-3.5 text-neutral-400"
				viewBox="0 0 24 24"
				fill="none"
				stroke="currentColor"
				stroke-width="1.75"
				aria-hidden="true"
			>
				<circle cx="12" cy="12" r="9" />
				<path d="M12 7v5l3 2" stroke-linecap="round" stroke-linejoin="round" />
			</svg>
			{time}
		</span>
	</header>

	<div class="relative mb-5 size-14">
		<img
			src="/avatar.jpg"
			alt="Osemudiame Okojie"
			class="size-14 rounded-2xl object-cover"
			width="56"
			height="56"
		/>
		<span
			class="absolute -right-0.5 -bottom-0.5 size-3.5 rounded-full border-2 border-white bg-green-500"
			aria-hidden="true"
		></span>
	</div>

	<div class="mb-6">
		<h1 class="text-lg font-semibold text-neutral-950">Osemudiame Okojie</h1>
		<p class="text-neutral-500">Software engineer at DoorDash</p>
	</div>

	<p class="text-[15px] leading-relaxed text-neutral-500">
		Hey, I'm Osemudiame, a software engineer at
		<a
			href="https://www.doordash.com"
			rel="external"
			class="font-medium text-neutral-950 transition hover:text-neutral-600"
		>
			DoorDash
		</a>
		and founder of
		<a
			href="https://terraglance.com"
			rel="external"
			class="font-medium text-neutral-950 transition hover:text-neutral-600"
		>
			Terraglance
		</a>, based in Helsinki, Finland <span aria-label="Finland">🇫🇮</span> exploring AI, startups, and
		technology.
	</p>

	<p class="mt-8 text-sm text-neutral-400">
		{#if copied}
			<span class="text-neutral-600">Copied to clipboard</span>
		{:else}
			Press <kbd>C</kbd> to copy my email
		{/if}
	</p>

	<p class="mt-4 text-sm text-neutral-400">
		{#each links as link, i (link.label)}
			{#if i > 0}<span class="text-neutral-300"> · </span>{/if}
			<a href={link.href} rel="external" class="text-neutral-500 transition hover:text-neutral-950">
				{link.label}
			</a>
		{/each}
	</p>
</main>
