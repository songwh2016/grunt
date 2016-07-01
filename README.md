
  grunt是一套前端自动化工具，一个基于nodeJs的命令行工具，一般用于：<br />
    * 压缩文件 
    * 合并文件 
    * 简单语法检查 <br />
  因为grunt是基于nodeJs的，所以首先各位需要安装nodeJS环境。<br />
  npm install -g grunt-cli <br />
  这条命令将会把grunt命令植入系统路径，这样就能在任意目录运行他。<br />
  认识Gruntdile与package.json <br />
  package.json <br />
  这个文件用来存储npm模块的依赖项（比如我们的打包若是依赖requireJS的插件，这里就需要配置）
  然后，我们会在里面配置一些不一样的信息，比如我们上面的file，这些数据都会放到package中<br />
  { <br />
    "name": "demo",<br />
    "file": "zepto",<br />
    "version": "0.1.0",<br />
    "description": "demo",<br />
    "license": "MIT",<br />
    "devDependencies": {<br />
      "grunt": "~0.4.1",<br />
      "grunt-contrib-jshint": "~0.6.3",<br />
      "grunt-contrib-uglify": "~0.2.1",<br />
      "grunt-contrib-requirejs": "~0.4.1",<br />
      "grunt-contrib-copy": "~0.4.1",<br />
      "grunt-contrib-clean": "~0.5.0",<br />
      "grunt-strip": "~0.2.1"<br />
    },<br />
    "dependencies": {<br />
      "express": "3.x"<br />
    }<br />
  }<br />
  pGruntfile.js<br />
  我们需要在grunt目录下执行 npm install将相关的文件下载下来：<br />
  module.exports = function (grunt) { <br />
    // 项目配置 <br />
    grunt.initConfig({ <br />
      pkg: grunt.file.readJSON('package.json'), <br />
      uglify: { <br />
        options: { <br />
          banner: '/*! <%= pkg.file %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'<br />
        }, <br />
        build: { <br />
          src: 'src/<%=pkg.file %>.js', <br />
          dest: 'dest/<%= pkg.file %>.min.js' <br />
        } <br />
      } <br />
    }); <br />
    // 加载提供"uglify"任务的插件 <br />
    grunt.loadNpmTasks('grunt-contrib-uglify'); <br />
    // 默认任务<br />
    grunt.registerTask('default', ['uglify']); <br />
  }<br />
  然后运行 grunt命令后：  $ grunt<br />
  每一个gurnt都会需要这两个文件，并且很可能就只有这两个文件 <br />
  Gruntfile.js这个文件尤其关键，他一般干两件事情： <br />
  * 读取package信息
  * 插件加载、注册任务，运行任务（grunt对外的接口全部写在这里面）
  ###Gruntfile一般由四个部分组成
  * 包装函数
  这个包装函数没什么东西，意思就是我们所有的代码必须放到这个函数里面
  module.exports = function (grunt) {
  //你的代码
  }
  * 项目/任务配置
  我们在Gruntfile一般第一个用到的就是initConfig方法配置依赖信息<br />
  
  pkg: grunt.file.readJSON('package.json')<br />
  
  这里的 grunt.file.readJSON就会将我们的配置文件读出，并且转换为json对象,
  然后我们在后面的地方就可以采用pkg.XXX的方式访问其中的数据了,
  值得注意的是这里使用的是underscore模板引擎，所以你在这里可以写很多东西。<br />
  
  uglify是一个插件的，我们在package依赖项进行了配置，这个时候我们为系统配置了一个任务<br />
  uglify（压缩）<br />
  加载实际函数<br />
  grunt.loadNpmTasks('grunt-contrib-uglify');<br />
  配置任务/grunt.initConfig<br />
  grunt.initConfig({<br />
      concat: {<br />
          //这里是concat任务的配置信息<br />
      },<br />
      uglify: {<br />
          //这里是uglify任务的配置信息<br />
      },<br />
      //任意非任务特定属性<br />
      my_property: 'whatever',<br />
      my_src_file: ['foo/*.js', 'bar/*.js']<br />
  });<br />
  我们使用grunt的时候，主要工作就是配置任务或者创建任务，实际上就是做一个事件注册，然后由我们触发之，所以grunt的核心还是事件注册
  每次运行grunt时，我们可以指定运行一个或者多个任务，通过任务决定要做什么，比如我们同时要压缩和合并还要做代码检查。<br />
  
  grunt.registerTask('default', ['jshint','qunit','concat','uglify']);<br />

