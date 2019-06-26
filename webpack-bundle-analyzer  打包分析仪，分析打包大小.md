`webpack-bundle-analyzer`  打包分析仪，分析打包大小	



构建命令行工具

`commander.js`

`inquirer.js`

`chalk`让命令多色输出

`dll refrence Plugin` 用来提高打包速度

editorconfig

`eslintrc.js`

@符号错误	 `babel-eslint`

只有一个return的组件推荐使用function

```js
const Setting () => {
    <div>
        404
    </div>
}
```

i += 1

componentDidmont()提前

gitignore

```js
/no-mo
/dist/
    
```

有报错阻止提交

```js
npm i husky --save-dev
```

package.json

```js
"husky": {
    "hooks" : {
        "pre-commit": "npm run lint:fix"
    }
}
```

hosts