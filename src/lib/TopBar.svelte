<script lang="ts">
	import TopAppBar, { Row, Section } from '@smui/top-app-bar';
	import IconButton from '@smui/icon-button';
</script>

<TopAppBar
	variant="short" collapsed
>
	<Row>
		<Section align="end" toolbar>
			
			<IconButton class="material-icons" aria-label="Sensors" href="/sensors"
			>sensors</IconButton
			>
			<IconButton class="material-icons" aria-label="Control" href="/control"
			>tune</IconButton
			>
		</Section>
	</Row>
</TopAppBar>