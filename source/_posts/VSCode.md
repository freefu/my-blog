---
title: VSCode配置相关
layout: post
tag: VSCode
---

记录一下自己使用vscode时的一些配置

<!--more-->

##### 用户代码片段
```javascript
{
	"Print to console": {
		"prefix": "vue",
		"body": [
			"<!-- $0 -->",
			"<template>",
			"  <div></div>",
			"</template>",
			"",
			"<script>",
			"export default {",
			"  data () {",
			"    return {",
			"    }",
			"  },",
			"  components: {},",
			"  computed: {},",
			"  methods: {},",
			"  created () { },",
			"  mounted () { }",
			"}",
			"",
			"</script>",
			"",
			"<style scoped lang=\"stylus\">",
			"</style>",
			""
		],
		"description": "Log output to console"
	}
}
```

##### 安装插件
- Atom One Light Theme
- Beautify
- Chinese
- Eslint
- language-stylus
- prettier-Code formatter
- Vetur

##### setting.json
```
{
  "editor.fontSize": 12,
  // Specifies the location of snippets in the suggestion widget
  "editor.snippetSuggestions": "top",
  // Controls whether format on paste is on or off
  "editor.formatOnPaste": true,
  "editor.tabSize": 2,
  "explorer.confirmDelete": false,
  // 是否开启eslint检测
  "eslint.enable": true,
  // 文件保存时，是否自动根据eslint进行格式化
  "eslint.autoFixOnSave": true,
  "eslint.validate": [
      "javascript",
      "javascriptreact",
      {
          "language": "html",
          "autoFix": true
      },
      {
          "language": "vue",
          "autoFix": true
      }
  ],
  "workbench.colorTheme": "Atom One Light",
}
```