---
layout:     post
title:      "gulp"
subtitle:   "how to use gulp"
date:       2017-11-20 11:17:04
author:     "Thq"
header-img: "img/contact-bg.jpg"
catalog: true
tags:
    - gulp
---
> 主要记录了gulp的用法便于自己忘记的时候过来复习~

<br>首先全局安装gulp
> npm install --global gulp

<br>作为项目开发依赖安装
> npm install --save-dev gulp

<br>常见的一些用法
```
//请求一个gulp模块
let gulp = require('gulp');	
let gulp_concat = require('gulp-concat');		//JS合并
let gulp_babel = require('gulp-babel');			//es6 转 es5
let gulp_compressed = require('gulp-uglify');	//JS压缩
let gulp_sass = require('gulp-sass');			//sass编译
let gulp_minify = require('gulp-minify-css');	//css压缩
let rename = require('gulp-rename');			//文件重命名
let connect = require('gulp-connect');			//server
let plumber = require('gulp-plumber');			//水管工 针对自动保存的插件
let clean = require('gulp-clean');				//删除dist
//使用gulp定义任务
gulp.task('copy-index', () => {
	//1.将文件读入内存
	//2.写到该路径下
	//gulp.src ==> 读取参数指定文件
	//gulp.pipe ==> 前一个函数结果作为下一个函数的输入
	//gulp.dest ==> 将输入文件流输出到指定路径
	gulp.src('./src/index.html')
		.pipe(gulp.dest('./dist'));
})

gulp.task('copy-html', () => {
	gulp.src('./src/html/*.html')
		.pipe(gulp.dest('./dist/html'));
})

gulp.task('copy-resource', () => {
	gulp.src('./resource/**/*.*')
		.pipe(gulp.dest('./dist/resource'));
})

gulp.task('copy-lib', () => {
	gulp.src('./vendor/**/*.*')
		.pipe(gulp.dest('./dist/vendor'))
})

gulp.task('copy', ['copy-index', 'copy-html', 'copy-resource', 'copy-lib'])

gulp.task('js', () => {
	gulp.src('./src/script/**/*.*')
		.pipe(gulp_concat('output.js'))
		.pipe(gulp.dest('./dist/js'))
		.pipe(gulp_babel())
		.pipe(gulp_compressed())
		.pipe(rename('output.min.js'))
		.pipe(gulp.dest('./dist/js'))
})

gulp.task('sass', () => {
	gulp.src('src/style/**/*.scss')
		//.pipe(plumber())
		.pipe(gulp_sass())
		.pipe(gulp.dest('./dist/css'))
		.pipe(gulp_minify())
		.pipe(rename(filename => {
			filename.basename += '.min';
		}))
		.pipe(gulp.dest('./dist/css'))
})

gulp.task('reload', () => {
	gulp.src('./dist/**/*.html')
		.pipe(connect.reload());
})

gulp.task('watch', () => {
	gulp.watch('./src/style/**/*.scss', ['sass']);
	gulp.watch('./src/index.html', ['copy']);
	gulp.watch('./src/script/**.js', ['js']);
	gulp.watch('./dist/**/*.*', ['reload']);
})

gulp.task('build', ['copy', 'js', 'sass'])
gulp.task('clean', () => {
	gulp.src('./dist')
		.pipe(clean());
})

gulp.task('server', () => {
	connect.server({
		root: './dist',
		livereload: true
	})
})

gulp.task('default', ['watch', 'server']);
```