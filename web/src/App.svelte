<script>
  import 'animate.css';
  import 'bulma/css/bulma.css';
  import Faucet from './Faucet.svelte';
</script>

<svelte:head>
  <meta name="twitter:card" content="summary" />
  <meta name="twitter:site" content="@ethoprotocol" />
  <meta name="twitter:creator" content="@ethoprotocol" />
  <meta property="og:url" content="https://testnetfaucet.ethoprotocol.com" />
  <meta property="og:title" content="A faucet to provide ETHO HC coins - the testnet coins of Etho Protocol" />
  <meta property="og:description" content="Etho HC Testnet work coins can be obtained here in order to interact with the test net." />
  <meta property="og:image" content="https://testnetfaucet.ethoprotocol.com/favicon.png" />
  <link
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    rel="stylesheet"
  />
</svelte:head>

<Faucet />
