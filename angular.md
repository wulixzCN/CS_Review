# `Angular`

1.安装Node.js，安装的方式有很多，请参考Node.js官网（http://nodejs.org/download）

```php
brew install node
```

2.接着安装TypeScript，请运行下列npm命令：

```js
npm install -g typescript
```

3.然后安装angular-cli，Angular提供了一个命令行工具angular-cli，它能让用户通过命令行创建和管理项目，比如创建项目，添加新的控制器等。想要安装angular-cli，只要运行下列命令：

```c#
npm install -g @angular/cli
```

安装成功后，你就可以使用ng命令了

(报错watchman)那你还需要安装一个叫watchman的工具，它是帮助angular-cli监听文件系统的变化，建议使用HomeBrew工具安装它，命令如下：

```js
brew install watchman
```

---

> 创建angular项目

1. 打开一个新的文件夹

   ```
   sudo ng new angularDemo01
   cd angularDemo01
   sudo npm install
   ng serve --open
   ```

2.配置新组件

```js
ng g component components/home
components/home  : app下创建components文件夹，创建 home组件
```

