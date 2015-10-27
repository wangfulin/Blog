#  gulp的简介

## 什么是gulp
gulp是在你的开发过程中用来帮你自动化一些麻烦耗时的工作的工具集。比如在web开发过程中，它能帮你完成css预处理，js文件整合，压缩，实时重加载等等。并且它还和目前市场上主流的IDE整合，无论是在PHP,.NET,Node.js,Java等等开发者都很喜欢用它。拥有超过1700中插件，gulp能够让你从繁琐的构建系统中解脱出来，真正的开始干活。

## 怎样开始

1. 安装[Node.js](http://www.infoq.com/cn/articles/nodejs-npm-install-config/)
2. 创建一个项目，比如说gulptest
3. 在控制台项目路径下，npm init，跟着步骤创建package.json文件
4. npm install --global gulp，使gulp命令可以在命令行中使用。
5. npm install --save-dev gulp，在项目上安装gulp模块，同时将 gulp添加到package.json的dev-dependencies依赖中。
6. 比如我们需要压缩css文件，先安装需要的模块npm install --save-dev gulp-minify-css
7. 在项目路径下创建gulpfile.js

		var gulp = require('gulp');
    	var minifyCss = require('gulp-minify-css');
    
    	gulp.task('minify-css', function() {
     	 return gulp.src('styles/*.css')
    		.pipe(minifyCss({compatibility: 'ie8'}))
    		.pipe(gulp.dest('dist'));
   		});

8. 运行gulp minify-css命令

## gulp API
跳转到：[gulp.src](#gulpsrc) | [gulp.dest](#gulpdest) | [gulp.task](#gulptask) |[gulp.watch](#gulpwatch)

<h3 id="gulpsrc">gulp.src</h3>
- Emits files matching provided glob or an array of globs. Returns a stream of Vinyl files that can be piped to plugins.

		gulp.src('client/templates/*.jade')
  			.pipe(jade())
 			.pipe(minify())
  			.pipe(gulp.dest('build/minified_templates'));

- 根据路径找到文件然后把它放在流里面pipe到gulp插件
		
		gulp.src('client/js/**/*.js') // Matches 'client/js/somedir/somefile.js' and resolves `base` to `client/js/`
  			.pipe(minify())
  			.pipe(gulp.dest('build'));  // Writes 'build/somedir/somefile.js'

		gulp.src('client/js/**/*.js', { base: 'client' })
  			.pipe(minify())
  			.pipe(gulp.dest('build'));  // Writes 'build/js/somedir/somefile.js'

<h3 id="gulpdest">gulp.dest</h3>
- Can be piped to and it will write files. Re-emits all data passed to it so you can pipe to multiple folders. Folders that don't exist will be created.

		gulp.src('./client/templates/*.jade')
  			.pipe(jade())
  			.pipe(gulp.dest('./build/templates'))
 			.pipe(minify())
  			.pipe(gulp.dest('./build/minified_templates'));

<h3 id="gulptask">gulp.task</h3>
- Define a task using [Orchestrator](https://github.com/robrich/orchestrator).

		gulp.task('somename', function() {
  			// Do stuff
		});

- An array of tasks to be executed and completed before your task will run.

		gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  			// Do stuff
		});

- You can also omit the function if you only want to run the dependency tasks:

		gulp.task('build', ['array', 'of', 'task', 'names']);

- Accept a callback

		// run a command in a shell
		var exec = require('child_process').exec;
		gulp.task('jekyll', function(cb) {
 			// build Jekyll
 			exec('jekyll build', function(err) {
   			if (err) return cb(err); // return error
    		cb(); // finished task
  			});
		});

- Return a stream

		gulp.task('somename', function() {
 			var stream = gulp.src('client/**/*.js')
    			.pipe(minify())
    			.pipe(gulp.dest('build'));
  			return stream;
		});

- Return a promise

		var Q = require('q');

		gulp.task('somename', function() {
  			var deferred = Q.defer();

  			// do async stuff
  			setTimeout(function() {
    			deferred.resolve();
  			}, 1);

  			return deferred.promise;
		});

- 默认情况下task中的任务是同步进行的，所以，如果需要在保证某一项任务在其他之前可以按照如下操作：

		var gulp = require('gulp');

		// takes in a callback so the engine knows when it'll be done
		gulp.task('one', function(cb) {
    	// do stuff -- async or otherwise
    	cb(err); // if err is not null and not undefined, the run will stop, and note that it failed
		});

		// identifies a dependent task must be complete before this one begins
		gulp.task('two', ['one'], function() {
   		 // task 'one' is done now
		});

		gulp.task('default', ['one', 'two']);
<h3 id="gulpwatch">gulp.watch</h3>
- Watch files and do something when a file changes. This always returns an EventEmitter that emits change events.

- Names of task(s) to run when a file changes, added with gulp.task()

		var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
		watcher.on('change', function(event) {
  			console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
		});

## 常用的gulp plugins
1. 编译Sass [gulp-ruby-sass](https://github.com/sindresorhus/gulp-ruby-sass)
2. 自动添加前缀 [gulp-autoprefixer](https://github.com/Metrime/gulp-autoprefixer)
3. 压缩CSS [gulp-minify-css](https://github.com/jonathanepollack/gulp-minify-css)
4. JSHint [gulp-jshint](https://github.com/wearefractal/gulp-jshint)
5. Concatenation [gulp-concat](https://github.com/wearefractal/gulp-concat)
6. Uglify [gulp-uglify](https://github.com/terinjokes/gulp-uglify)
7. 图片压缩 [gulp-imagemin](https://github.com/sindresorhus/gulp-imagemin)
8. 实时重加载 [gulp-livereload](https://github.com/vohof/gulp-livereload)
9. 图片缓存从而只压缩改变的图片 [gulp-cache](https://github.com/jgable/gulp-cache/)
10. Notify of changes [gulp-notify](https://github.com/mikaelbr/gulp-notify)
11. 清除文件干净地构建 [del](https://www.npmjs.org/package/del)

### 参考链接：

1. [github-gulp](https://github.com/gulpjs/gulp)
2. [gulp-documentation](https://github.com/gulpjs/gulp/blob/master/docs/README.md)