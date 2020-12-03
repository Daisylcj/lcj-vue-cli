###
【1】在github上上传自己封装好的vue基础模版（项目lcj-cli-vue）
###
【2】生成一个项目（lcj-cli）用来存放脚手架工具代码
【3】运行 npm init -y 生成package.json文件
【4】运行 npm i commander download-git-repo 安装我们需要的模块commander，download-git-repo。
//commander: tj大神开发的可以对命令行做解析的库，可以解析我们在命令行输入的如lcj-vue-cli -v或lcj-vue-cli init <projectName>等命令。
//download-git-repo: 下载git仓库代码的库。
【5】在项目根目录创建index.js文件，写入相应的处理逻辑
    #! /usr/bin/env node

    const program = require('commander');
    const download = require('download-git-repo');
    //version 版本号
    //name 新项目名称
    program.version('1.0.0', '-v, --version')
        .command('init <projectName>')
        .action((projectName) => {
            console.log('clone template ...lcj');
            download('github:Daisylcj/lcj-cli-vue', projectName, function (err) {
                console.log(err ? 'Error' : 'Success')
            })
        });
    program.parse(process.argv);
    .#! /usr/bin/env node是执行这个文件时使用node方式执行
    .program.version是解析别人输入lcj-vue-cli -v时输出的内容: 1.0.0
    .command解析输入lcj-vue-cli init vue my-vue-project，init后面两个参数，一个模板名，一个项目名
    .action是根据上面的两个参数做相应的逻辑处理，判断模板名，去相应的git仓库下载代码。download的第一个参数下载地址不是填我们git的网址，按照我的格式填就行，第二个参数是生成的项目名，第三个参数是错误的回调执行函数。
【6】在package.json文件中加入，这一步是我们在命令行lcj-vue-cli的时候执行的文件。
    "bin": {
        "jkc-cli": "index.js"
    },
###
【7】把脚手架工具上传到npm
    npm login
    npm publish
    !!!每次更新发布记得修改版本号
###
【8】测试
    npm i lcj-vue-cli -g
    lcj-vue-cli -v
    lcj-vue-cli init my-vue-project
