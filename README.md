# gulp-s3 [![NPM version][npm-image]][npm-url]

> s3 plugin for [gulp](https://github.com/wearefractal/gulp)

## Usage

First, install `gulp-s3-gzip` as a development dependency:

```shell
npm install --save-dev gulp-s3-gzip
```

Setup your aws.json file
```javascript
{
  "key": "AKIAI3Z7CUAFHG53DMJA",
  "secret": "acYxWRu5RRa6CwzQuhdXEfTpbQA+1XQJ7Z1bGTCx",
  "bucket": "dev.example.com",
  "region": "eu-west-1"
}
```

Then, use it in your `gulpfile.js`:
```javascript
var s3 = require("gulp-s3-gzip");

aws = JSON.parse(fs.readFileSync('./aws.json'));
gulp.src('./dist/**')
    .pipe(s3(aws));
```

## API


#### options.headers

Type: `Array`          
Default: `[]`

Headers to set to each file uploaded to S3

```javascript
var options = { headers: {'Cache-Control': 'max-age=315360000, no-transform, public'} }
gulp.src('./dist/**', {read: false})
    .pipe(s3(aws, options));
```

#### options.removeGzipExtension

Type: `Boolean`
Default: `false`

When handling uploading assets to S3, the build should be able to decide whether or not to remove any `.gz` extensions from gzipped files. The decision to add this flag stems from the documentation Amazon provides on [Serving Compressed Files from Amazon S3](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/ServingCompressedFiles.html#CompressedS3). To summarize:

> Amazon S3 doesn't automatically compress files as web servers do. If you want to serve compressed content and 
> you're using Amazon S3 as your origin, you need to store compressed and uncompressed versions of your files in 
> your Amazon S3 bucket. You also need to develop your application to intercept viewer requests and change the 
> request URL based on whether the request includes an Accept-Encoding: gzip header.

#### options.gzippedOnly

Type: `Boolean`          
Default: `false`

Only upload files with .gz extension, additionally it will remove the .gz suffix on destination filename and set appropriate Content-Type and Content-Encoding headers.

```javascript
var gulp = require("gulp");
var s3 = require("gulp-s3-gzip");
var gzip = require("gulp-gzip");
var options = { gzippedOnly: true };

gulp.src('./dist/**')
.pipe(gzip())
.pipe(s3(aws, options));

});
```

## License

[MIT License](http://en.wikipedia.org/wiki/MIT_License)

[npm-url]: https://npmjs.org/package/gulp-s3-gzip
[npm-image]: https://badge.fury.io/js/gulp-s3-gzip.png
