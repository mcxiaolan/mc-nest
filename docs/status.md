# 服务器状态
## IPv6
> 从 [Minecraft Server Status](https://mcsrvstat.us/bedrock/play.ltya.top) 的 [API](https://api.mcsrvstat.us/bedrock/3/play.ltya.top) 获取。

<div id="server-status"></div>
<script>
    async function fetchServerStatus() {
        try {
            const response = await fetch('https://api.mcsrvstat.us/bedrock/3/play.ltya.top');
            const data = await response.json();

            let serverStatus = `
                    <strong>IP 地址:</strong> play.ltya.top<br>
                    <strong>端口:</strong> ${data.port}<br>
                    <strong>服务器状态:</strong> ${data.online ? '在线' : '离线'}<br>
                `;

            if (data.online) {
                serverStatus += `
                        <strong>服务器 MOTD:</strong> ${data.motd.clean}<br>
                        <strong>当前在线玩家:</strong> ${data.players.online} / ${data.players.max}<br>
                        <strong>服务器版本:</strong> ${data.version}<br>
                        <strong>游戏模式:</strong> ${data.gamemode}<br>
                        <strong>地图名称:</strong> ${data.map.clean}<br>
                    `;
            }
            document.getElementById('server-status').innerHTML = serverStatus;
        } catch (error) {
            document.getElementById('server-status').innerHTML = `<p class="error">无法获取服务器状态信息，请稍后再试。<br>`;
        }
    }

    fetchServerStatus();
</script>

## IPv4 (湖南长沙)

> 从 [BlackBE云黑](https://blackbe.work) 提供的 [MCBE-Server-Motd](https://motdbe.blackbe.work/iframe.html?ip=cs-v4.play.ltya.top&port=19100&dark=true&join_open=true) 获取。

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="500px" height="195px" scrolling=no src="//motdbe.blackbe.work/iframe.html?ip=cs-v4.play.ltya.top&port=19100&dark=true&join_open=true"></iframe>

