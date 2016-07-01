# grunt
是一套前端自动化工具，一个基于nodeJs的命令行工具，一般用于：
1、压缩文件
2、合并文件
3、简单语法检查
因为grunt是基于nodeJs的，所以首先各位需要安装nodeJS环境。
npm install -g grunt-cli
这条命令将会把grunt命令植入系统路径，这样就能在任意目录运行他。
认识Gruntdile与package.json
package.json
这个文件用来存储npm模块的依赖项（比如我们的打包若是依赖requireJS的插件，这里就需要配置）
然后，我们会在里面配置一些不一样的信息，比如我们上面的file，这些数据都会放到package中
{
  "name": "demo",
  "file": "zepto",
  "version": "0.1.0",
  "description": "demo",
  "license": "MIT",
  "devDependencies": {
    "grunt": "~0.4.1",
    "grunt-contrib-jshint": "~0.6.3",
    "grunt-contrib-uglify": "~0.2.1",
    "grunt-contrib-requirejs": "~0.4.1",
    "grunt-contrib-copy": "~0.4.1",
    "grunt-contrib-clean": "~0.5.0",
    "grunt-strip": "~0.2.1"
  },
  "dependencies": {
    "express": "3.x"
  }
}
Gruntfile.js
我们需要在grunt目录下执行 npm install将相关的文件下载下来：
module.exports = function (grunt) {
  // 项目配置
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.file %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%=pkg.file %>.js',
        dest: 'dest/<%= pkg.file %>.min.js'
      }
    }
  });
  // 加载提供"uglify"任务的插件
  grunt.loadNpmTasks('grunt-contrib-uglify');
  // 默认任务
  grunt.registerTask('default', ['uglify']);
}
然后运行 grunt命令后：  $ grunt
每一个gurnt都会需要这两个文件，并且很可能就只有这两个文件
Gruntfile.js这个文件尤其关键，他一般干两件事情：
① 读取package信息
② 插件加载、注册任务，运行任务（grunt对外的接口全部写在这里面）
Gruntfile一般由四个部分组成
① 包装函数
这个包装函数没什么东西，意思就是我们所有的代码必须放到这个函数里面
module.exports = function (grunt) {
//你的代码
}
② 项目/任务配置
我们在Gruntfile一般第一个用到的就是initConfig方法配置依赖信息

pkg: grunt.file.readJSON('package.json')

这里的 grunt.file.readJSON就会将我们的配置文件读出，并且转换为json对象

然后我们在后面的地方就可以采用pkg.XXX的方式访问其中的数据了
值得注意的是这里使用的是underscore模板引擎，所以你在这里可以写很多东西

uglify是一个插件的，我们在package依赖项进行了配置，这个时候我们为系统配置了一个任务
uglify（压缩）
加载实际函数
grunt.loadNpmTasks('grunt-contrib-uglify');
配置任务/grunt.initConfig
grunt.initConfig({
    concat: {
        //这里是concat任务的配置信息
    },
    uglify: {
        //这里是uglify任务的配置信息
    },
    //任意非任务特定属性
    my_property: 'whatever',
    my_src_file: ['foo/*.js', 'bar/*.js']
});
我们使用grunt的时候，主要工作就是配置任务或者创建任务，实际上就是做一个事件注册，然后由我们触发之，所以grunt的核心还是事件注册
每次运行grunt时，我们可以指定运行一个或者多个任务，通过任务决定要做什么，比如我们同时要压缩和合并还要做代码检查

grunt.registerTask('default', ['jshint','qunit','concat','uglify']);

