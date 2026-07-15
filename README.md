# 国泰航空奖励里程航班查询插件 / Cathay Award Flight Finder

这是一个用于国泰航空预订页面的 Chrome 浏览器扩展。它会在页面中加入一个奖励里程航班查询面板，帮助你更方便地查看可兑换航班信息。

This is a Chrome browser extension for Cathay Pacific booking pages. It adds an award flight finder panel to help you check redeemable flight options more easily.

## 功能特点 / Features

- 在支持的国泰航空页面中添加查询面板。
  - Adds a search panel on supported Cathay Pacific pages.
- 将上次输入的航线、日期、乘客数量和语言设置保存在浏览器本地。
  - Saves the user's last entered route, date, passenger count, and language preference locally in the browser.
- 支持批量查询多个日期以及用逗号分隔的机场代码。
  - Supports batch searches across multiple dates and comma-separated airport codes.
- 在数据可用时显示可用舱位、航班详情以及预计所需的国泰里程。
  - Displays available cabins, flight details, and estimated Cathay points when the data is available.

## 安装说明 / Installation

1. 打开 chrome://extensions/。
   1. Open chrome://extensions/.
2. 开启“开发者模式”。
   2. Enable Developer mode.
3. 点击“加载已解压的扩展程序”。
   3. Click "Load unpacked".
4. 选择当前项目目录。
   4. Select the current project folder.

## 上架打包 / Store Packaging

如需上传到 Chrome Web Store，请将扩展文件夹内容打包。打包内容应包含 manifest.json、content.js、icons/、README.md 和 PRIVACY.md。

To upload the extension to the Chrome Web Store, package the extension folder contents. The package should include manifest.json, content.js, icons/, README.md, and PRIVACY.md.

