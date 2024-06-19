# React Native代码片段

## VSCode常用代码片段

```js
{
	"create rn view temp": {
		"scope": "javascript",
		"prefix": "rnview",
		"body": [
			"import React, { memo } from 'react'",
			"import { StyleSheet, View } from 'react-native'",
			"",
			"const index = memo((props) => {",
			" return (",
			" <View style={styles.view} >",
			"",
			" </View>",
			" )",
			"})",
			"",
			"const styles = StyleSheet.create({",
			" view: {",
			" flex: 1,",
			" }",
			"})",
			"",
			"export default index"
		],
		"description": "创建React Native组件"
	},
	"create rn paage temp": {
		"scope": "javascript",
		"prefix": "rnpage",
		"body": [
			"import React, { useState, useEffect, } from 'react'",
			"import { observer } from 'mobx-react-lite'",
			"import { StyleSheet, View } from 'react-native'",
			"",
			"const index = observer((props) => {",
			" return (",
			" <View style={styles.view} >",
			"",
			" </View>",
			" )",
			"})",
			"",
			"const styles = StyleSheet.create({",
			" view: {",
			" flex: 1,",
			" }",
			"})",
			"",
			"export default index"
		],
		"description": "创建React Native页面"
	},
}
```