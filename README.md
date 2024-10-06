# Formsnap

<!-- automd:badges license name="formsnap" color="blue" github="svecosystem/formsnap" -->

[![npm version](https://flat.badgen.net/npm/v/formsnap?color=blue)](https://npmjs.com/package/formsnap)
[![npm downloads](https://flat.badgen.net/npm/dm/formsnap?color=blue)](https://npmjs.com/package/formsnap)
[![license](https://flat.badgen.net/github/license/svecosystem/formsnap?color=blue)](https://github.com/svecosystem/formsnap/blob/main/LICENSE)

<!-- /automd -->

[![](https://dcbadge.vercel.app/api/server/fdXy3Sk8Gq?style=flat)](https://discord.gg/fdXy3Sk8Gq)

The goal of this library is to make working with the already incredible [sveltekit-superforms](https://github.com/ciscoheat/sveltekit-superforms) even more pleasant, by wrapping it with accessible form components.

## Installation

```bash
npm i formsnap sveltekit-superforms <your-schema-library>
```

## Usage

You'll handle the initial Superforms setup just as you normally would, where you define a schema and return the form from your load function.

#### 1. Define a Zod schema

```ts
// schemas.ts
import { z } from "zod";
export const settingsFormSchema = z.object({
	email: z.string().email(),
	bio: z.string().max(250).optional(),
	marketingEmails: z.boolean().default(true),
	language: z.enum(["en", "es", "fr"]).default("en"),
	theme: z.enum(["light", "dark"]).default("light"),
});
```

#### 2. Return the form from your load function

```ts
// +page.server.ts
import { superValidate } from "sveltekit-superforms";
import { zod } from "sveltekit-superforms/adapters";
import type { PageServerLoad } from "./$types";
import { settingsFormSchema } from "./schemas";

export const load: PageServerLoad = async () => {
	return {
		form: await superValidate(zod(settingsFormSchema)),
	};
};
```

#### 3. Construct the form in your page

```svelte
<script lang="ts">
	import { Field, Label, FieldErrors, Control, Description, Fieldset, Legend } from "formsnap";
	import { settingsFormSchema } from "./schemas";
	import { superForm } from "sveltekit-superforms";
	import { zodClient } from "sveltekit-superforms/adapters";

	export let data;

	const form = superForm(data.form, {
		validators: zodClient(settingsFormSchema),
	});

	const { form: formData, enhance } = form;
</script>

<form method="POST" use:enhance>
	<Field {form} name="email">
		<Control let:attrs>
			<Label>Email</Label>
			<input type="email" {...attrs} bind:value={$formData.email} />
		</Control>
		<Description>We'll provide critical updates about your account via email.</Description>
		<FieldErrors />
	</Field>

	<Field {form} name="bio">
		<Control let:attrs>
			<Label>Bio</Label>
			<textarea bind:value={$formData.bio} {...attrs} />
		</Control>
		<FieldErrors />
	</Field>

	<Field {form} name="language">
		<Control let:attrs>
			<Label>Language</Label>
			<select {...attrs} bind:value={$formData.language}>
				<option value="en">English</option>
				<option value="es">Spanish</option>
				<option value="fr">French</option>
			</select>
		</Control>
		<FieldErrors />
	</Field>

	<Field {form} name="marketingEmails">
		<Control let:attrs>
			<Label>Receive marketing emails from us</Label>
			<input type="checkbox" {...attrs} bind:checked={$formData.marketingEmails} />
		</Control>
		<FieldErrors />
	</Field>

	<Fieldset {form} name="theme">
		<Legend>Select your theme</Legend>
		{#each ["light", "dark"] as theme}
			<Control let:attrs>
				<input {...attrs} type="radio" bind:group={$formData.theme} value={theme} />
				<Label>{theme}</Label>
			</Control>
		{/each}
		<FieldErrors />
	</Fieldset>

	<button type="submit">Submit</button>
</form>
```

Check out [Formsnap.dev](https://formsnap.dev) for more documentation.

## Sponsors

This project is supported by the following beautiful people/organizations:

<p align="center">
  <a href="https://github.com/sponsors/huntabyte">
    <img src='https://cdn.jsdelivr.net/gh/huntabyte/static/sponsors.svg' alt="Logos from Sponsors" />
  </a>
</p>

## License

<!-- automd:contributors license=MIT author="huntabyte" github="svecosystem/formsnap" -->

Published under the [MIT](https://github.com/svecosystem/formsnap/blob/main/LICENSE) license.
Made by [@huntabyte](https://github.com/huntabyte) and [community](https://github.com/svecosystem/formsnap/graphs/contributors) 💛
<br><br>
<a href="https://github.com/svecosystem/formsnap/graphs/contributors">
<img src="https://contrib.rocks/image?repo=svecosystem/formsnap" />
</a>

<!-- /automd -->

## Community

Join the Discord server to ask questions, find collaborators, or just say hi!

<a href="https://discord.gg/fdXy3Sk8Gq" alt="Svecosystem Discord community">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://invidget.switchblade.xyz/fdXy3Sk8Gq">
  <img alt="Svecosystem Discord community" src="https://invidget.switchblade.xyz/fdXy3Sk8Gq?theme=light">
</picture>
</a>
