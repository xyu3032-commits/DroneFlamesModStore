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

## notices.json（公告，游戏内与官网共用）

本仓库根目录的 `notices.json` 是**公告的唯一数据源**：游戏主菜单的「公告」和官网首页的公告区都读这一份文件。**发新公告 = 往这份文件里加一条，不需要重打包游戏、不需要改官网。**

```json
{
	"notices_version": 1,
	"notices": [
		{
			"id": "2026-11-25-v44-release",
			"title_zh": "公告标题（中文）",
			"title_en": "Notice title (English)",
			"body_zh": "公告正文（中文）。",
			"body_en": "Notice body (English).",
			"date": "2026-11-25",
			"level": "normal",
			"expires": null,
			"min_game_version": null
		}
	]
}
```

字段说明：

- `id`：唯一标识，同一条公告不要改 id（游戏靠 id 判断已读、官网靠它对齐）。建议用 `日期-短名` 格式。
- `title_zh / title_en`、`body_zh / body_en`：中英双份标题与正文，游戏和官网按当前语言取对应字段（其他语言回退英文）。
- `date`：发布日期 `YYYY-MM-DD`，列表按日期倒序，新公告放数组前面即可。
- `level`：`normal` 普通公告（只在公告列表里）；`important` 重要公告（游戏内进主菜单会弹窗一次，官网会突出显示）。
- `expires`：过期日期 `YYYY-MM-DD` 或 `null`（永不过期）。到期后游戏内不再显示，官网转入历史置灰。
- `min_game_version`：最低游戏版本如 `"0.33.5.44"` 或 `null`（不限）。低于此版本的游戏不显示这条——「请先更新到 x.xx 才能看到后续内容」这类公告用。
