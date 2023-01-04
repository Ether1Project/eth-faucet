<script>
  import 'animate.css';
  import 'bulma/css/bulma.css';
  import Faucet from './Faucet.svelte';
</script>

<svelte:head>
  <meta name="twitter:card" content="summary" />
  <meta name="twitter:site" content="@ethoprotocol" />
  <meta name="twitter:creator" content="@ethoprotocol" />
  <meta property="og:url" content="https://faucet.ethoprotocol.com" />
  <meta property="og:title" content="A faucet to provide coins - coins of Etho Protocol family" />
  <meta property="og:description" content="Coins can be obtained here in order to interact with the chain." />
  <meta property="og:image" content="https://faucet.ethoprotocol.com/favicon.png" />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/fork-awesome@1.2.0/css/fork-awesome.min.css" integrity="sha256-XoaMnoYC5TH6/+ihMEnospgm0J1PM/nioxbOUdnM8HY=" crossorigin="anonymous">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">
    <script src='https://cdn.jsdelivr.net/gh/Ether1Project/ethocomponents@main/component.js' crossorigin="anonymous"></script>

    <script>let ethocomponent_menu='' +
            '<li class="nav-item"><a class="nav-link" href="https://docs.ethoprotocol.com/install-metamask><i class="fa fa-home"></i>How to setup ETHO in Metamask</a></li>' +
            '';
    </script>

</svelte:head>

<Faucet />
