<script>
  import { onMount } from 'svelte';
  import { getAddress } from '@ethersproject/address';
  import { setDefaults as setToast, toast } from 'bulma-toast';

  let address = null;
  let faucetInfo = {
    account: '0x0000000000000000000000000000000000000000',
    network: 'testnet',
    bgimg: '',
    payout: 1,
    faucetstr: "",
    faucetstr1: "",
    faucetlnk1: "",
    faucetstr2: "",
    faucetlnk2: "",
    faucetstr3: "",
    faucetlnk3: "",
    faucetstr4: "",

  };

  $: document.title = `ETHO HC Faucet`;

  onMount(async () => {
    const res = await fetch('/api/info');
    faucetInfo = await res.json();
    faucetInfo.payout=faucetInfo.payout/100;
    // We load different backgrounds depending on the network spec
    switch(faucetInfo.network) {
        case 'etho':
            faucetInfo.bgimg='background_etho.jpg';
            faucetInfo.faucetstr="ETHO Faucet";
            faucetInfo.faucetstr1="How to setup ETHO in Metamask";
            faucetInfo.faucetlnk1="https://docs.ethoprotocol.com/install-metamask"
            faucetInfo.faucetstr2="Etho Stats";
            faucetInfo.faucetlnk2="https://stats.ethoprotocol.com"
            faucetInfo.faucetstr3="Etho Explorer";
            faucetInfo.faucetlnk3="https://explorer.ethoprotocol.com";
            faucetInfo.faucetstr4="ETHO per request";
            document.title = `ETHO Faucet`;
            break;
        case 'ethotestnet':
            faucetInfo.bgimg='background_ethotestnet.jpg';
            faucetInfo.faucetstr="ETHO HC Faucet";
            faucetInfo.faucetstr1="How to setup ETHO HC in Metamask";
            faucetInfo.faucetlnk1="https://ethoprotocol.com/2022/12/11/etho-hc-test-network-launched";
            faucetInfo.faucetstr2="Etho HC Stats";
            faucetInfo.faucetlnk2="https://testnetstats.ethoprotocol.com"
            faucetInfo.faucetstr3="Etho HC Explorer";
            faucetInfo.faucetlnk3="https://testnetexplorer.ethoprotocol.com";
            faucetInfo.faucetstr4="ETHO HC per request";
            document.title = `ETHO HC Faucet`;
            break;
        default:
            faucetInfo.bgimg='background.jpg';
            document.title = `Ethereum Faucet`;
            break;
        }
        console.log(faucetInfo);
  });

  setToast({
    position: 'bottom-center',
    dismissible: true,
    pauseOnHover: true,
    closeOnClick: false,
    duration: 5000,
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
  <section class="hero is-info is-fullheight" style="background-image: url({faucetInfo.bgimg})">
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
              <span><b>{faucetInfo.faucetstr}</b></span>
            </a>
            <a class="navbar-item" href={faucetInfo.faucetlnk1}>
              <span class="icon">
                <i class="fa fa-arrow-circle-right" />
              </span>
              <span><b>{faucetInfo.faucetstr1}</b></span>
            </a>
            <a class="navbar-item" href={faucetInfo.faucetlnk2}>
              <span class="icon">
                <i class="fa fa-eye" />
              </span>
              <span><b>{faucetInfo.faucetstr2}</b></span>
            </a>
            <a class="navbar-item" href={faucetInfo.faucetlnk3}>
              <span class="icon">
                <i class="fa fa-at" />
              </span>
              <span><b>{faucetInfo.faucetstr3}r</b></span>
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
            Receive {faucetInfo.payout} {faucetInfo.faucetstr4}
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
    background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5));
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

</style>
