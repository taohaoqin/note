先安装node.js 官方下载http://nodejs.cn/
在Windows7上安装commitizen  执行命令↓（用cmd或Git bash）
    npm install -g commitizen
在安装changelog，是生成changelog的工具
    npm install -g conventional-changelog
在安装conventional-changelo脚手架工具
    npm install -g conventional-changelog-cli
查看是否安装成功
    npm ls -g -depth=0
查看到都安装成功后在运行下面命令
    commitizen init cz-conventional-changelog --save --save-exact
    执行后会多出个node_modules文件夹
最后在创建一个package.json文件 执行命令 如果有该文件了可以忽略这步
    npm init --yes
都安装完毕后，在该文件夹测试一下，把当前文件下测试，把该文件变成.git文件git add . 后执行git cz 出现下面内容
? Select the type of change that you're committing: (Use arrow keys)
? feat:     A new feature 
  fix:      A bug fix 
  docs:     Documentation only changes 
  style:    Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc) 
  refactor: A code change that neither fixes a bug nor adds a feature 
  perf:     A code change that improves performance 
  test:     Adding missing tests or correcting existing tests...
  
  而不是跳转到其他地方的内容 跳转到其他内容的话就是失败 

  成功后在git bash下执行
     echo '{"path":"cz-conventional-changelog"}'>~/.czrc
     把上面的文件配置成全局
之后在其他地方创建测试文件 把该文件变成.git文件后执行测试 依旧出现上面的内容就表示成功了

弄了整整一天，安装了无数次到最后可算是弄出来了，一开始的时候就出不来 后来出来了但一直没办法弄成全局的，想在哪个文件弄必须在该文件按个node_modules 和package.json文件，想弄成全局的，避免以后这么麻烦，后来查了很久，结果在安装了这两个文件后也出不了 把别的同学都难倒了，弄了一下午，最后没办法，把今天某个时间段的文件夹全删了，包括node.js也删了重新装，最后再执行了一遍安装命令后 终于成功了，随便建个测试文件也不用安装node module和yupackage.json文件也能运行出来了。

 commit message 的作用
 1.提供更多的历史信息，方便快速浏览
 2.可以过滤某些commit（比如文档改动），便于快速查找信息
 3.可以直接从commit生成Change log
  commit message的格式（必须包括header，body，footer，header是必须的不可以省略）
 <type>(<scope>): <subject>// 空一行<body>// 空一行<footer>
 其中type是必须的，scope是可选的。subject是必须的
 1.type用于说明 commit 的类别，只允许使用下面7个标识。
   feat：新功能（feature）
   fix：修补bug
   docs：文档（documentation）
   style： 格式（不影响代码运行的变动）
   refactor：重构（即不是新增功能，也不是修改bug的代码变动）
   test：增加测试
   chore：构建过程或辅助工具的变动
2.scope：用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同
3.subject：对本次目的剪短描述，主要采用动词 ，一般为一般时态

  body:Provide a longer description of the change,一般为项目的具体描述
  footer： Are there any breaking changes?  是否作出重大改变，默认为N         
  Does this change affect any open issues? 一般需要一个issuse所对应的编号。





