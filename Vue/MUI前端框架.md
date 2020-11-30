### MUI前端框架

> MUI不同于Mint UI ，MUI只是开发出来的一套代码片段，提供了配套的样式、配套的HTML代码段，类似于BootStrap，任何项目都可以使用；而Mint UI是真正意义的组件库，使用Vue技术封装出来的组件，只适用与Vue项目

#### MUI的使用
- 不能npm下载，需要在[github地址]("https://github.com/dcloudio/mui")手动下载并import导入
- 下载的包从dist文件夹导入，使用案例参考examples\hello-mui\examples文件夹下的源码
- 在项目src目录下创建lib，用于放置拷贝过来的包，再将dist文件夹复制到该文件夹
- 在main.js中导入MUI的样式表`import './lib/mui/css/mui.min.css'`

