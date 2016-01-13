# winston-logrotate

[![Build Status](https://travis-ci.org/juttle/winston-logrotate.svg)](https://travis-ci.org/juttle/winston-logrotate)

## Overview

Winston's default [file
transport](https://github.com/flatiron/winston/blob/master/lib/winston/transports/file.js)
rotation creates new logs daily, or if the file exceeds a max size, but it does
not expunge old logs. This means that an additional module must be used to
delete old log files.

`winston-logrotate` provides a transport with full support for size based
rotation, compression and pruning log files.

`winston-logrotate` uses
[logrotate-stream](https://www.npmjs.org/package/logrotate-stream) under the
covers, which is used to perform compression/rotation and winston's
`common.log` to get timestamps, colorization, and log formatting.

## Options

| Field           | Required      | Description  |
| --------------- |-------------- | ----------------------------------- |
| file      | yes | The filename of the logfile to write output to.      |
| colorize  | no  | Boolean flag indicating if we should colorize output.|
| timestamp | no  | Boolean flag indicating if we should prepend output with timestamps (default false). If function is specified, its return value will be used instead of timestamps. |
| json      | no  | If true, messages will be logged as JSON (default true). |
| size      | no  | The max file size of a log before rotation occurs. Supports 1024, 1k, 1m, 1g. Defaults to 100m |
| keep      | no  | The number of rotated log files to keep (including the primary log file). Defaults to 5 |
| compress  | no  | Optionally compress rotated files with gzip. Defaults to true. |

## Usage

Require:

```
require('winston-logrotate');
```

Create a rotate transport:

```
var rotateTransport = new winston.transports.Rotate({
        file: '/tmp/my.log', // this path needs to be absolute
        colorize: false,
        timestamp: true,
        json: false,
        max: '100m',
        keep: 5,
        compress: true
});

```

and add it to default winston logger:

```
winston.add(rotateTransport)
```

or create a custom logger using the transport:

```
var logger = new (winston.Logger)({ transports: [rotateTransport] });
```
