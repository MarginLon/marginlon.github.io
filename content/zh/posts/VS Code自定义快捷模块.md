---
title: "VS Code自定义快捷模块"
description:
toc: 
authors:
tags: 
  - visual studio code
categories:
  - visual studio code
series:
  - visual studio code
date: '2020-12-08T18:02:39+08:00'
lastmode: "2020-12-08T18:02:39+08:00"
featuredImage: 
featuredVideo: 
draft: false
---
## VS Code自定义快捷模块
---
```js
//1.<ctrl>+<shift>+<p>
//2. snippets
//3. 新建
/*4. args:
prefix：使用代码段的快捷入口
body：需要设置的代码放在这里,字符串间换行的话使用\r\n换行符隔开.如果值里包含特殊字符需要进行转义,多行代码以","分隔(在引号后面写逗号)
$0：定义最终光标位置
$1：定义第一次光标位置，按tab键可进行快速切换, 还可以有 $2, $3, $4, $5 ...
description：代码段描述,在使用智能感知时的描述
 * /
```

```json
// 自定义(vue页面结构)
 {
     "vue": {
    "prefix": "vue-tem", // 设置的快捷入口
    "body": [ // 模块内容
        "<!-- $1 -->", // 光标
        "<template>",
        "  <div>",
        "     $2",
        "  </div>",
        "</template>",
        "",
        "<script>",
        "  export default {",
        "    name: '',",
      "    components: {",
      "         ",
      "    },",
      "    props: {",
      "       ",
      "    },",
      "    data() {",
      "      return {",
      "          ",
      "      }",
      "    },",
      "    computed: {",
      "        ",
      "    },",
      "    created() {",
      "        ",
        "    },",
      "    methods: {",
      "        ",
        "    },",
        "    watch: {",
        "       ",
        "    },",
        "  }",
            "</script>",
            "",
            "<style scoped lang=''>",
            "    ",
            "</style>"
    ],
    "description": "Vue templet" // 说明
 }
}
```

```json
{
	"h5 sample":{
		"prefix": "MYH5",
		"body": [
			"<!DOCTYPE html>",
			"<html>",
			"<head>",
			"\t<meta charset=\"UTF-8\">",
			"\t<meta name=\"keywords\" content=\"\">",
			"\t<meta name=\"description\" content=\"\">",
			"\t<meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",
			"\t<title>Document</title>",
			"\t<!-- CSS -->",
			"\t<link rel=\"stylesheet\" type=\"text\\css\" href=\"\">",
			"\t<style>",
			"",
			"\t</style>",
			"</head>",
			"",
			"<body>",
			"\t<!-- JS -->",
			"\t<script>",
			"",		
			"\t</script>",
			"</body>",
			"",
			"</html>"
		],
		"description": "create HTML5 templete"
	}
}
```