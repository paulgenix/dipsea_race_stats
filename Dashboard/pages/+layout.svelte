<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
</script>


<EvidenceDefaultLayout data={data} hideBreadcrumbs=true hideSidebar={false}  title="Dipsea Race Stats" builtWithEvidence={false} neverShowQueries={true} hideTOC=true >
	<slot slot="content" />
</EvidenceDefaultLayout>
