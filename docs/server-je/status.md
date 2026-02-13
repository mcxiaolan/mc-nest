# 服务器状态

<div id="mc-status-container" style="margin: 20px 0; min-height: 180px;">
    <style>
        .mc-query-card {
            background: var(--md-code-bg-color, #ffffff);
            border: 1px solid rgba(0,0,0,0.1);
            border-radius: 4px;
            padding: 16px;
            color: var(--md-typeset-color, #313131);
            font-family: 'Segoe UI', "Noto Sans SC", sans-serif;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            max-width: 500px;
        }
        [data-md-color-scheme="slate"] .mc-query-card {
            background: #2e303e;
            color: #e0e0e0;
            border-color: #444;
        }
        .mc-query-header { display: flex; align-items: center; gap: 12px; margin-bottom: 12px; }
        .mc-query-icon { width: 48px; height: 48px; border-radius: 4px; image-rendering: pixelated; background: #444; }
        .mc-query-title h3 { margin: 0 !important; font-size: 1.1rem !important; border-bottom: none !important; }
        .mc-query-status { font-size: 0.8rem; font-weight: bold; }
        .mc-query-motd {
            background: #1a1a1a; padding: 10px; border-radius: 4px;
            font-family: 'Consolas', monospace; margin-bottom: 12px;
            font-size: 0.85rem; line-height: 1.4; color: #fff; overflow-x: auto;
        }
        .mc-query-tags { display: flex; gap: 6px; margin-top: 5px; }
        .mc-query-tag { font-size: 0.7rem; padding: 1px 6px; border-radius: 4px; font-weight: bold; color: white; }
        .mc-query-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; font-size: 0.8rem; opacity: 0.8; }
        .mc-online { color: #4cae4f; }
        .mc-offline { color: #ff5555; }
    </style>

    <div id="mc-display-area" class="mc-query-card">
        <p style="text-align:center; font-size:0.9rem;">正在同步服务器状态...</p>
    </div>

    <script>
        (function() {
            const TARGET_ADDR = "play.getmc.top:1300";
            const BE_ICON_BASE64 = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJAAAACQCAMAAADQmBKKAAAALVBMVEWtelSZj4uXZ0aAVTlhPSllQCpSpTU8hSd5UDVQMSBmQSxsw0mPYUJ2TTSG1WKGxbn6AAABTUlEQVR42u3aQW7CQBQEUZOYEAzJ/Y/LAWoWpdHIsKheou/PA29aY2//yA/yQHgVZ8wezmyBAgUKFChQoECBPgzEj+4IZ26I+Xpu5swAtCOrQNysQAeyCsTNgQIFChQoUKBA7wexxV2Qu4jph9/IoMKyxRG0i5h+qED8Gwk6RMwNChQoUKBAgQIF+kTQF3JFOLMhph9yD68agH4RAzL9kHt41QBkFhFkbhD38KpAgQIFChQoUKCzQeyHHGKvMz/DtMoBiP2QQ+x1cyDuGYC4yFT6OZDZHChQoECBAgUKdDaIMUTzLNucVZqfoUDmWbY5q1wGMofJpuQHChQoUKBAgQK9HzR5NIn8IZxRz7tXgZ4IZ9Tz7lUgc3SsDpwDBQoUKFCgQIFOBpn3D009Zdgh1RGnef9wDsQOqUDmBYA50NytDxQoUKBAgQIFOhn0Aun11yhsDFNEAAAAAElFTkSuQmCC";

            async function fetchStatus() {
                try {
                    const [je, be] = await Promise.allSettled([
                        fetch(`https://api.mcsrvstat.us/3/${TARGET_ADDR}`).then(r => r.json()),
                        fetch(`https://api.mcsrvstat.us/bedrock/3/${TARGET_ADDR}`).then(r => r.json())
                    ]);

                    const jeData = je.status === 'fulfilled' ? je.value : null;
                    const beData = be.status === 'fulfilled' ? be.value : null;
                    render(jeData, beData);
                } catch (e) {
                    document.getElementById('mc-display-area').innerHTML = '<p class="mc-offline">状态更新失败</p>';
                }
            }

            function render(je, be) {
                const isJE = je && je.online;
                const isBE = be && be.online;
                const area = document.getElementById('mc-display-area');

                if (!isJE && !isBE) {
                    area.innerHTML = '<p class="mc-offline" style="text-align:center">● 服务器目前离线</p>';
                    return;
                }

                const icon = (isJE && je.icon) ? je.icon : (isBE ? BE_ICON_BASE64 : '');
                const motd = isJE ? je.motd.html.join('<br>') : (be ? be.motd.html.join('<br>') : '在线');

                area.innerHTML = `
                    <div class="mc-query-header">
                        <img src="${icon}" class="mc-query-icon" alt="Server Icon">
                        <div class="mc-query-title">
                            <span class="mc-online">● 在线</span>
                            <h3>${TARGET_ADDR}</h3>
                            <div class="mc-query-tags">
                                ${isJE ? '<span class="mc-query-tag" style="background:#3c8527">Java '+je.version+'</span>' : ''}
                                ${isBE ? '<span class="mc-query-tag" style="background:#1e87f0">Bedrock '+be.version+'</span>' : ''}
                            </div>
                        </div>
                    </div>
                    <div class="mc-query-motd">${motd}</div>
                    <div class="mc-query-grid">
                        <div>在线人数: <strong>${isJE ? je.players.online : be.players.online}/${isJE ? je.players.max : be.players.max}</strong></div>
                        <div>协议版本: ${isJE ? je.protocol.version : be.protocol.version}</div>
                    </div>
                `;
            }

            fetchStatus();
        })();
    </script>
</div>

## Java 版
地址: getmc.top  
备用地址1: play.getmc.top:1300  
备用地址2: fun.getmc.top:1300

## 基岩版
地址: play.getmc.top:1300  
备用地址: fun.getmc.top:1300