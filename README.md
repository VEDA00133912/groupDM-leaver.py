# groupDM-leaver
Discord.py-selfを使用したグループDM自動退出セルボ<br>
元コードは06君
## 目的
大量のグループDMに追加されるスパムへの対処<br>
Selfbotは規約違反のため使う際は自己責任で
## Discord.py-selfを使った全グループDMの自動退出
1.ターミナル開いてこれする
```
pip install discord.py-self python_dotenv
```
2.実行
```
python main.py
```
3.少し待って`ユーザー名#---- is online！`と表示されると自動退出が開始します
4.退出が完了するとコンソールに`◯個のグループDMの退出が完了しました`と表示されます<br>
※もし退出したくないグループDMがある場合はそのグループのIDをDM_IDSに入れておくとそこは除外されます

### 全部のグループDMから脱退する場合

```python
import discord  
import os
from dotenv import load_dotenv
load_dotenv()

client = discord.Client()

TOKEN = os.getenv("TOKEN")

@client.event
async def on_ready():
    print(f"{client.user} is online！")
    group_dms = [dm for dm in client.private_channels if isinstance(dm, discord.GroupChannel)]
    left_count = 0
    for dm in group_dms:
            await dm.leave()
            left_count += 1
    print(f"{left_count}件のグループDMからの退出が完了しました")

client.run(TOKEN)
```
### 一部のグループからは抜けたくない場合

```python
DM_IDS = ["12345678", "23456789"]

@client.event
async def on_ready():
    print(f"{client.user} is online！")
    group_dms = [dm for dm in client.private_channels if isinstance(dm, discord.GroupChannel)]
    left_count = 0
    for dm in group_dms:
        if str(dm.id) not in DM_IDS:
            await dm.leave()
    print(f"{left_count}件のグループDMからの退出が完了しました")
```
