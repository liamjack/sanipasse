<script type="ts">
	import { Alert, Icon, Row, Col } from 'sveltestrap';
	import { findCertificateError } from '$lib/detect_certificate';
	import type { CommonCertificateInfo } from '$lib/common_certificate_info';
	import Certificate2ddocDetails from './_Certificate2ddocDetails.svelte';
	import CertificateDgcDetails from './_CertificateDGCDetails.svelte';
	export let info: CommonCertificateInfo;
	export let with_fullscreen = false;
	$: error = findCertificateError(info);
	$: source = info.source;
</script>

<Alert color={error ? 'warning' : 'info'} fade={false}>
	{#if with_fullscreen}
		<a style="float:right" href="/fullscreen#{encodeURIComponent(info.code)}">
			<Icon name="arrows-fullscreen" />
		</a>
	{/if}
	<Row>
		<div class="col-sm-0 col-md-3 text-center align-middle emoji">
			{info.type === 'vaccination' ? '💉' : '🧪'}
		</div>
		<Col sm="12" md="9">
			<h4>
				{info.type === 'vaccination'
					? 'Vaccin'
					: info.type === 'test'
					? 'Test de dépistage'
					: 'Certificat de rétablissement'}
			</h4>
			<p>
				👤
				<span class="first_name">{info.first_name.toLocaleLowerCase()}</span>
				<span class="last_name">{info.last_name}</span>
			</p>
			<p>🎂 Né(e) le {info.date_of_birth.toLocaleDateString('fr')}</p>
		</Col>
	</Row>
	<Row>
		{#if error}
			<p class="error">⚠️ <strong>{error}</strong></p>
		{/if}
		<details class="px-0 px-sm-2 px-md-4 px-lg-5 ">
			{#if source.format === '2ddoc'}
				<Certificate2ddocDetails certificate={source.cert} />
			{:else}
				<CertificateDgcDetails certificate={source.cert} />
			{/if}
		</details>
	</Row>
</Alert>

<style>
	.first_name {
		text-transform: capitalize;
	}
	p {
		margin-bottom: 0.5rem;
	}
	.emoji {
		font-size: 3.5em;
		margin: auto;
	}
</style>
