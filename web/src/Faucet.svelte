<script>
  import { onMount } from 'svelte';
  import { getAddress } from '@ethersproject/address';
  import { setDefaults as setToast, toast } from 'bulma-toast';

  let address = null;
  let faucetInfo = {
    account: '0x0000000000000000000000000000000000000000',
    network: 'testnet',
    payout: 1,
  };

  $: document.title = `ETHO HC ${capitalize(faucetInfo.network)} Faucet`;

  onMount(async () => {
    const res = await fetch('/api/info');
    faucetInfo = await res.json();
  });

  setToast({
    position: 'bottom-center',
    dismissible: true,
    pauseOnHover: true,
    closeOnClick: false,
    animate: { in: 'fadeIn', out: 'fadeOut' },
  });

  async function handleRequest() {
    try {
      address = getAddress(address);
    } catch (error) {
      toast({ message: error.reason, type: 'is-warning' });
      return;
    }

    const res = await fetch('/api/claim', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        address,
      }),
    });

    let { msg } = await res.json();
    let type = res.ok ? 'is-success' : 'is-warning';
    toast({ message: msg, type });
  }

  function capitalize(str) {
    const lower = str.toLowerCase();
    return str.charAt(0).toUpperCase() + lower.slice(1);
  }
</script>

<main>
  <section class="hero is-info is-fullheight">
    <div class="hero-head">
      <nav class="navbar">
        <div class="container">
          <div class="navbar-brand">
            <a class="navbar-item" href="https://ethoprotocol.com">
              <span class="icon">
                <i class="fa fa-home" />
              </span>
              <span><b>Etho Protocol</b></span>
            </a>

            <a class="navbar-item" href=".">
              <span class="icon">
                <i class="fa fa-bath" />
              </span>
              <span><b>ETHO HC Faucet</b></span>
            </a>
            <a class="navbar-item" href="https://ethoprotocol.com/2022/12/11/etho-hc-test-network-launched/">
              <span class="icon">
                <i class="fa fa-books" />
              </span>
              <span><b>How to setup ETHO HC in Metamask</b></span>
            </a>
            <a class="navbar-item" href="https://testnetstats.ethoprotocol.com">
              <span class="icon">
                <i class="fa fa-browser" />
              </span>
              <span><b>Etho HC Stats</b></span>
            </a>
            <a class="navbar-item" href="https://testnetexplorer.ethoprotocol.com">
              <span class="icon">
                <i class="fa fa-signal-strong" />
              </span>
              <span><b>Etho HC Explorer</b></span>
            </a>
          </div>
          <div id="navbarMenu" class="navbar-menu">
            <div class="navbar-end">
              <span class="navbar-item">
                <a
                  class="button is-white is-outlined"
                  href="https://discord.gg/MFn9Tmz"
                >
                  <span class="icon">
                    <i class="fa fa-comments" />
                  </span>
                  <span>Join our Discord</span>
                </a>
              </span>
            </div>
          </div>
        </div>
      </nav>
    </div>

    <div class="hero-body">
      <div class="container has-text-centered">
        <div class="column is-6 is-offset-3">
          <h1 class="title">
            Receive {faucetInfo.payout} ETHO HC per request
          </h1>
          <h2 class="subtitle">
            Serving from {faucetInfo.account}
          </h2>
          <div class="box">
            <div class="field is-grouped">
              <p class="control is-expanded">
                <input
                  bind:value={address}
                  class="input is-rounded"
                  type="text"
                  placeholder="Enter your address"
                />
              </p>
              <p class="control">
                <button
                  on:click={handleRequest}
                  class="button is-primary is-rounded"
                >
                  Request
                </button>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</main>

<style>
  .hero.is-info {
    background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)),
      url('/background.jpg') no-repeat center center fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;

  }
  .hero .subtitle {
    padding: 3rem 0;
    line-height: 1.5;
  }
  .box {
    border-radius: 19px;
  }
  button.is-primary {
      background-color: #7a022a;
      border-color: transparent;
      color: #fff;
  }
</style>
