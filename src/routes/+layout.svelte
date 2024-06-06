<script>

    // Importing Carbon design css
    import "carbon-components-svelte/css/all.css";
    import {Theme} from 'carbon-components-svelte'

</script>


	<!-- Theme changer -->
	<Theme
		render="select"
		persist
		persistKey="carbon-theme"
		select={{
			themes: ["g10", "g80"],
			labelText: "Select a carbon theme",
			inline: true,
		}}
	/>
<main>
    <slot />
</main>


<style>
    main {
        max-width: 99dvw;
        margin: auto;
    }
</style>