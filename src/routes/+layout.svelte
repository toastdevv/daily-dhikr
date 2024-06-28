<script>
	import '../app.css';
	import { browser } from '$app/environment';

	if (browser) {
		document.body.dir = 'rtl';
	}
</script>

<slot />
