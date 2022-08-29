<script>
  import platformLogo from '../assets/minecraftblock.png';

  let serverAddress = "164.92.192.179",
      serverPort = "25565";

  const trackingMinecraftServer = (async () => {
		const response = await fetch('https://mcapi.us/server/status?ip=' + serverAddress + '&port=' + serverPort)
    return await response.json()
	})()

  function showCopying(option) {
    switch(option) {
      case 'true': {
        document.getElementById('copy').style.display = 'block';
      } break;
      case 'false': {
        document.getElementById('copy').style.display = 'none';
      } break;
    }
  }

  function copyToClipboard() {
    const elem = document.createElement('textarea');
    elem.value = '164.92.192.179:25565';
    document.body.appendChild(elem);
    elem.select();
    document.execCommand('copy');
    document.body.removeChild(elem);
    alert('Copied to clipboard!');
  }
</script>

    <div class="serverPack" on:click={copyToClipboard}>
      {#await trackingMinecraftServer}
        <div class="loading">
          <div class="loading__text">loading...</div>
          <center><div class="loading__anim"></div></center>
        </div>
      {:then data}
        {#if !data.online}
          <img src={platformLogo} alt="Minecraft" class='d-sm-inline-block'>

          <div class='d-inline-block ms-2'>
              <div id="serverStatus" style='background-color: #b02525;'></div> <h4 style='color: #b02525;'>OFFLINE</h4>

              <p id='serverName'>{data.server.name}</p>
              <p id='serverIp'>{serverAddress}:{serverPort}</p>

              <div class="progress">
                <div class="progress-bar" role="progressbar" style="width: 0%;" aria-valuemin={0} aria-valuemax={100} aria-valuenow={0}/>
              </div>
        
              <p id='serverPlayersTxt'>no-one is online right now</p>
          </div>
        {:else}
          <img src={platformLogo} alt="Minecraft" class='d-sm-inline-block'>

          <div class='d-inline-block ms-2'>
              <div id="serverStatus" style='background-color: #46bc4f;'></div> <h4 style='color: #46bc4f;'>ONLINE</h4>

              <p id='serverName'>{data.server.name}</p>
              <p id='serverIp'>{serverAddress}:{serverPort}</p>
          </div>

          <div class="progress">
            <div class="progress-bar" role="progressbar" style="width: {data.players.now * 100 / data.players.max}%;" aria-valuemin={0} aria-valuemax={data.players.max} aria-valuenow={data.players.now}/>
          </div>

          <p id='serverPlayersTxt'>{data.players.now > 0 ? data.players.now + ' players online' : 'no-one is online right now'}</p>
        {/if}
      
      {:catch error}
        <div class="loading">
          <div class="loading__text">AN ERROR OCCURED</div>
          <center><div class="loading__anim"></div></center>
        </div>
      {/await}
    </div>