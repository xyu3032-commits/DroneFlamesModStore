# DroneFlames Mod 商店源索引

本仓库是 DroneFlames 游戏内「下载模组」商店的**总入口**。游戏打开商店时，先读取本仓库根目录的 `index.json`，再按里面的链接逐个拉取各 mod 仓库的目录合并展示。

**上新 mod / 加新仓库 / 加新分类，全是改远程数据，不需要重打包游戏。**

## index.json（源索引）

```json
{
	"index_version": 1,
	"sources": [
		{"id": "modern_warfare", "name": "ModernWarfareMod",
		 "url": "https://raw.githubusercontent.com/xyu3032-commits/ModernWarfareMod/main/mod_catalog/catalog.json"}
	]
}
```

- `sources` 每项是一个 mod 仓库：`id` 唯一标识，`name` 显示在商店卡片的来源处，`url` 指向该仓库 `catalog.json` 的 raw 直链。
- 游戏逐源拉取、合并展示；任一源拉取失败只跳过该源并在商店里标红，不影响其他源。
- **加新仓库 = 往 `sources` 里加一项**（该仓库需先放好自己的 `catalog.json`）。

## catalog.json（各仓库的模组目录，放在各自仓库里）

`mods` 数组，每条字段：`id / name / icon / version / size / download_url / sha256 / description`，外加可选字段 `category`。

- **在已有仓库上新 mod** = 往那个仓库的 `catalog.json` 加一条（.dfmod 先传到该仓库的 Release，`download_url` 填直链）。
- **加新分类** = 在条目里写一个新 `category` 名即可。分类完全由数据定义，游戏不写死名单；商店顶部会按数据里出现过的分类自动生成筛选。不写 `category` 的条目只出现在「全部」里。

## 当前源

| id | 名称 | 目录 |
|---|---|---|
| modern_warfare | ModernWarfareMod | [ModernWarfareMod 仓库](https://github.com/xyu3032-commits/ModernWarfareMod) 的 `mod_catalog/catalog.json`（现代战争：合并版 + 五阵营模块包） |

玩家也可以在商店里点「添加源」，粘贴任意 `catalog.json` 直链作为个人自定义源（只存本地，与本索引无关）。
