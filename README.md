# bespoke-asciidoc
> A [Bespoke.js](http://markdalgleish.com/projects/bespoke.js) presentation, built with [generator-bespoke](https://github.com/markdalgleish/generator-bespoke)

## Installation from bespoke generator

For installing asciidoc presentation inside bespoke, initially we have to create a bespoke presentation using yeoman generator. 

```bash
$ yo bespoke
```

Once created the presentation, we could see that the generator use jade as a default presentation language. For changing it to asciidoc, we have to add the needed libraries to the project. To do that, we have to add the *package.json* the folowing libraries: 

```json
"gulp-chmod": "^1.3.0",
"gulp-exec": "^2.1.2",
```

After introducing the libraries, we have to installing it: 

```bash
$ npm i
```

At time all libraries were installed, we will focus on the definition of the gulp tasks to on the one hand, perform the online conversion between adoc and html and, on the other hand create the *"watch"* functions. 

Staring with the conversion task, we have to define the following task inside the gulpfile.js:

```json
gulp.task('html', ['clean:html'], function() {
 return gulp.src('src/index.adoc')
  .pipe(isDist ? through() : plumber())
  .pipe(exec('bundle exec asciidoctor-bespoke
  -D public -T src/templates -o - src/index.adoc',
  { pipeStdout: true }))
  .pipe(exec.reporter({ stdout: false }))
  .pipe(rename('index.html'))
  .pipe(gulp.dest('dist'))
  .pipe(chmod(644))
  .pipe(gulp.dest('public'))
  .pipe(connect.reload());
});
```

Finally, we have to replace the following task: 

```json
gulp.watch('src/**/*.jade', ['html']);
```

with

```json
gulp.watch('src/**/*.adoc', ['html']);
```

And now, enjoy playing with ascidoc!!

## View slides locally

First, ensure you have the following installed:

1. [Node.js](http://nodejs.org)
2. [Bower](http://bower.io): `$ npm install -g bower`
3. [Gulp](http://gulpjs.com): `$ npm install -g gulp`

Then, install dependencies and run the preview server:

```bash
$ npm install && bower install
$ gulp serve
```
