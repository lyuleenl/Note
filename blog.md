 https://yleen.github.io/



token e0fabaff5b0d4a61e3a0469c0bf95523ebaff923











```undefined
npm install -g hexo-cli
hexo -v
```

```xml
hexo init <博客存放的目录>
cd <博客存放的目录>
npm install
hexo clean
hexo g -d 
hexo s //本地预览
hexo d //发布
```





```bash
deploy:
  type: git
  repository: https://github.com/godweiyang/godweiyang.github.io
  branch: master
```

```basemake
npm install hexo-deployer-git --save
```

注意 仓库名和账户名必须一样

asc