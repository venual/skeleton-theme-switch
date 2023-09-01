# Skeleton Theme Switcher Example



`file: src/+hooks.server.ts`
```js
import type { Handle } from '@sveltejs/kit'

export const handle: Handle = async ({ event, resolve }) => {
 


  let theme = '';

  const cookieTheme = event.cookies.get('theme');

  if (cookieTheme) {
    theme = cookieTheme;
  } else {
  // you can replace default theme from skeleton to whatever you want
    event.cookies.set('theme', 'skeleton', { path: '/', maxAge: 60 * 60 * 24 * 365 })
// replace here aswell if u want to have a different default
    theme = 'skeleton';
  }

  return resolve(event, {
    transformPageChunk: ({ html }) => html.replace('data-theme=""', `data-theme="${theme}"`),
  })
}
```


`file: src/app.html`
```html
<body data-sveltekit-preload-data="hover" data-theme="">
```

Change data theme to empty quotes


`file: src/routes/+page.server.ts`
```js
import type { Actions } from "@sveltejs/kit";


export const actions: Actions = {
  setTheme: async ({ url, cookies }) => {
    const theme = url.searchParams.get('theme');

    if (theme) {
      cookies.set('theme', theme, { path: '/', maxAge: 60 * 60 * 24 * 365 })
    }
  }
};
```



`file: src/routes/+page.svelte`
```js
<script lang="ts">
	import { enhance } from '$app/forms';
	import type { SubmitFunction } from '@sveltejs/kit';
	import '../app.postcss';

	const themes = [
		{ type: 'skeleton', name: 'Skeleton', icon: '💀' },
		{ type: 'wintry', name: 'Wintry', icon: '🌨️', badge: 'New' },
		{ type: 'modern', name: 'Modern', icon: '🤖' },
		{ type: 'rocket', name: 'Rocket', icon: '🚀' },
		{ type: 'seafoam', name: 'Seafoam', icon: '🧜‍♀️' },
		{ type: 'vintage', name: 'Vintage', icon: '📺' },
		{ type: 'sahara', name: 'Sahara', icon: '🏜️' },
		{ type: 'hamlindigo', name: 'Hamlindigo', icon: '👔' },
		{ type: 'gold-nouveau', name: 'Gold Nouveau', icon: '💫' },
		{ type: 'crimson', name: 'Crimson', icon: '⭕' }
	];

	const setTheme: SubmitFunction = ({ formData }) => {
		const theme = formData.get('theme')?.toString();

		if (theme) {
			document.body.setAttribute('data-theme', theme);
		}
	};
</script>

<div class="flex-none">
	<ul class="">
		<li>
			<ul class="p-2 bg-base-100 w-full overflow-y-scroll">
				<form method="POST" use:enhance={setTheme}>
					{#each themes as theme}
						<li>
							<button
								class="btn variant-filled-primary m-1"
								name="theme"
								value={theme.type}
								type="submit"
								formaction="/?/setTheme&theme={theme.type}"
							>
								<div class="flex flex-row space-x-2">
									<div class="">
										{theme.icon}
									</div>
									<p>{theme.name}</p>
								</div></button
							>
						</li>
					{/each}
				</form>
			</ul>
		</li>
	</ul>
</div>
```


and finally, be sure to import the themes in tailwind config, the +page.svelte file can be changed to whatever and you can do whatever with it, i just made it to test it and show how to do it
