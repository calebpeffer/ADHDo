<script lang="ts">
	// Docs
	import DocsLogoAnim from '$docs/DocsLogo/DocsLogoAnim.svelte';
	import Timer from '$lib/ADHDo/Timer.svelte';
	import TodoList from '$lib/ADHDo/TodoList.svelte';
	// Components
	import Avatar from '$lib/components/Avatar/Avatar.svelte';
	import SvgIcon from '$lib/components/SvgIcon/SvgIcon.svelte';

	// Fetch HTTP Data
	async function fetchHttpData(url: string, key?: string): Promise<any> {
		const http = await fetch(url);
		const res = await http.json();
		if (http.ok) {
			return key ? res[key] : res;
		} else {
			throw new Error(res);
		}
	}
	let contributors: Promise<any> = fetchHttpData('https://api.github.com/repos/skeletonlabs/skeleton/contributors');
</script>

<div>
	<!-- Hero -->
	<header class="card-glass-surface border-b border-black/5 dark:border-white/5 px-4 py-10 md:py-20">
		<Timer></Timer>
		<TodoList></TodoList>
	</header>

	<!-- SET TO DISPLAY NONE TEMPORARILY. REMOVE DISPLAY NONE TO SHOW -->
	<div class="hidden container max-w-[1200px] mx-auto px-4 py-10 md:py-20 space-y-20 ">
		<!-- Pre-Release Alert -->
		<!-- <aside class="alert">
			<i class="fa-solid fa-triangle-exclamation text-4xl" />
			<div class="alert-message">Skeleton is currently in beta. Expect breaking changes prior to v1.0. Please report bugs on GitHub.</div>
			<div class="alert-actions">
				<a href="https://github.com/skeletonlabs/skeleton/issues" target="_blank" rel="noreferrer" class="btn btn-filled">Report Issue</a>
			</div>
		</aside> -->

		<!-- Features -->
		<!-- <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
			<h2 class="sr-only">Features</h2>
			<div class="card card-glass-surface p-4 text-center space-y-4">
				<div class="bg-orange-500 w-20 mx-auto lg:-mt-10 aspect-square flex justify-center items-center rounded-token shadow">
					<SvgIcon name="svelte" fill="fill-white" width="w-10" height="h-10" />
				</div>
				<h4>Svelte Integration</h4>
				<p>
					Tightly coupled support for Svelte using the <a href="https://kit.svelte.dev/" target="_blank" rel="noreferrer">SvelteKit</a> app framework.
				</p>
			</div>
			<div class="card card-glass-surface p-4 text-center space-y-4">
				<div class="bg-sky-500 w-20 mx-auto lg:-mt-10 aspect-square flex justify-center items-center rounded-token shadow">
					<SvgIcon name="tailwind" fill="fill-white" width="w-10" height="h-10" />
				</div>
				<h4>Tailwind CSS Integration</h4>
				<p>Integrate with your design system using Tailwind utility classes to style components.</p>
			</div>
			<div class="card card-glass-surface p-4 text-center space-y-4">
				<div class="bg-primary-500 w-20 mx-auto lg:-mt-10 aspect-square flex justify-center items-center rounded-token shadow">
					<i class="fa-solid fa-swatchbook text-3xl" />
				</div>
				<h4>Dynamic Themes</h4>
				<p>Choose from a library of hand crafted preset themes or quickly generate your own in seconds.</p>
			</div>
			<div class="card card-glass-surface p-4 text-center space-y-4">
				<div class="bg-secondary-500 w-20 mx-auto lg:-mt-10 aspect-square flex justify-center items-center rounded-token shadow">
					<i class="fa-solid fa-screwdriver-wrench text-3xl" />
				</div>
				<h4>Powerful Utilities</h4>
				<p>Provides a feature rich UI toolkit for modern web apps with layouts, modals, and more.</p>
			</div>
		</section> -->

		<hr />

		<!-- Contributors & Sponsors -->
		<section class="grid grid-cols-1 md:grid-cols-2 gap-10 md:gap-20">
			<!-- Contributors -->
			<div class="text-center space-y-6">
				<!-- <h2>Contributors</h2>
				{#await contributors}
					<p class="flex flex-wrap justify-center">Loading contributors...</p>
				{:then response}
					<div class="grid grid-cols-8 md:grid-cols-10 gap-2">
						{#each response as c}
							<a href={c.html_url} target="_blank" rel="noreferrer" title={c.login}>
								<Avatar
									src={c.avatar_url}
									shadow="shadow-lg"
									border="border-2 border-transparent hover:!border-primary-500 cursor-pointer"
									width="w-full"
								/>
							</a>
						{/each}
					</div>
				{/await}
				<a class="btn btn-ghost-surface" href="/docs/contributions">
					<span>Contribution Guide</span>
					<span>&rarr;</span>
				</a>
			</div> -->
			<!-- Sponsors -->
			<!-- <div class="text-center space-y-6">
				<h2>Sponsors</h2>
				<a class="unstyled flex flex-col items-center space-y-4" href="https://www.brainandbonesllc.com/" target="_blank" rel="noreferrer">
					<img src="https://www.brainandbonesllc.com/svg/logo.svg" alt="Brain & Bones" class="h-[32px] mx-auto invert dark:invert-0" />
					<p class="text-sm">Brain & Bones</p>
				</a>
				<div class="grid grid-col-1 gap-4 max-w-[280px] mx-auto">
					<a class="btn btn-filled-primary" href="https://ko-fi.com/skeletonlabs" target="_blank" rel="noreferrer">Sponsor on Ko-Fi</a>
					<a class="btn btn-filled-primary" href="https://patreon.com/user?u=83786276" target="_blank" rel="noreferrer">
						Sponsor on Patreon
					</a>
				</div>
			</div> -->
		</section>

		<hr />

		<!-- Team -->
		<!-- <section class="text-center space-y-6">
			<h2>Meet The Team</h2>
			<ul class="grid grid-cols-1 md:grid-cols-2 gap-4 max-w-[800px] mx-auto">
				<li class="card card-glass-surface p-4 space-y-4">
					<Avatar src="https://avatars.githubusercontent.com/u/1509726?v=4" width="w-32" class="mx-auto" />
					<div>
						<h3>Chris Simmons</h3>
						<p>Co-founder and core maintainer.</p>
						<small class="!text-primary-500">(aka endigo9740)</small>
					</div>
					<div class="flex justify-center space-x-4">
						<a class="unstyled" href="https://github.com/endigo9740" target="_blank" rel="noreferrer">
							<i class="fa-brands fa-github text-xl" />
						</a>
						<a class="unstyled" href="https://twitter.com/endigodesign" target="_blank" rel="noreferrer">
							<i class="fa-brands fa-twitter text-xl" />
						</a>
						<a class="unstyled" href="https://www.linkedin.com/in/chris-simmons-8a523a23/" target="_blank" rel="noreferrer">
							<i class="fa-brands fa-linkedin text-xl" />
						</a>
					</div>
				</li>
				<li class="card card-glass-surface p-4 space-y-4">
					<Avatar src="https://avatars.githubusercontent.com/u/10255430?v=4" width="w-32" class="mx-auto" />
					<div>
						<h3>Trey Weir</h3>
						<p>Core member and founder of <a href="https://www.brainandbonesllc.com/" target="_blank" rel="noreferrer">Brain & Bones</a></p>
						<small class="!text-primary-500">(aka weirbones)</small>
					</div>
					<div class="flex justify-center space-x-4">
						<a class="unstyled" href="https://twitter.com/brain_and_bones" target="_blank" rel="noreferrer">
							<i class="fa-brands fa-twitter text-xl" />
						</a>
						<a class="unstyled" href="https://www.linkedin.com/company/brainandbones/" target="_blank" rel="noreferrer">
							<i class="fa-brands fa-linkedin text-xl" />
						</a>
					</div>
				</li>
			</ul>
			<div class="flex justify-center gap-4">
				<a class="btn btn-ghost-surface" href="mailto:admin@skeleton.dev">Reach Out</a>
				<a class="btn btn-filled-primary" href="https://www.skeletonlabs.co/" target="_blank" rel="noreferrer">
					<span>Skeleton Labs</span>
					<span>&rarr;</span>
				</a>
			</div>
		</section> -->
	</div>
</div>
