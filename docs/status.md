# 服务器状态

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="500px" height="195px" scrolling=no src="//motdbe.blackbe.work/iframe.html?ip=cs-v4.play.ltya.top&port=19100&dark=true&join_open=true"></iframe>

<div id="server-status"></div>
<script>
        async function fetchServerStatus() {
            try {
                const response = await fetch('https://api.mcsrvstat.us/bedrock/3/play.ltya.top');
                const data = await response.json();
                
                let serverStatus = `
                    <p><strong>IP 地址:</strong> ${data.ip}</p>
                    <p><strong>端口:</strong> ${data.port}</p>
                    <p><strong>服务器名称:</strong> ${data.hostname}</p>
                    <p><strong>服务器状态:</strong> ${data.online ? '在线' : '离线'}</p>
                `;
                
                if (data.online) {
                    serverStatus += `
                        <p><strong>服务器 MOTD:</strong> ${data.motd.clean}</p>
                        <p><strong>当前在线玩家:</strong> ${data.players.online}</p>
                        <p><strong>最大玩家数:</strong> ${data.players.max}</p>
                        <p><strong>服务器版本:</strong> ${data.version}</p>
                        <p><strong>游戏模式:</strong> ${data.gamemode}</p>
                        <p><strong>地图名称:</strong> ${data.map.clean}</p>
                    `;
                }
                document.getElementById('server-status').innerHTML = serverStatus;
            } catch (error) {
                document.getElementById('server-status').innerHTML = `<p class="error">无法获取服务器状态信息，请稍后再试。</p>`;
            }
        }

        fetchServerStatus();
    </script>
