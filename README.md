uri-counter
===============

URI counter

## Installation

~~~~
$ curl https://raw.githubusercontent.com/tkuchiki/uri-counter/master/uri-counter -o /usr/local/bin/uri-counter && chmod +x /usr/local/bin/uri-counter
~~~~

## usage

~~~~
$ ./request-counter
Usage: uri-counter -f FILE [option]

Options:
  -h, --help            show this help message and exit
  -f FILE, --file=FILE  path to log file
  -d DELIMITER, --delimiter=DELIMITER
                        field delimiter(default ' ')
  -r, --reverse         reverse the result of comparisons
  --sort-min            sort by min
  --sort-max            sort by max
  --sort-avg            sort by avg
  --sort-method         sort by method
  --no-headers          print no header line at all
~~~~

## sample (count and min, max, avg)

sample log file

~~~~
/api/test?foo=bar&ho=ge 0.1 GET
/api/test?ho=geee&foo=baar 0.2 GET
/api/foo 0.01 GET
/api/foo 0.03 POST
/api/foo 0.05 GET
/api/foo 0.07 POST
/api/foo 0.09 POST
/api/bar 0.08 GET
/api/bar 0.3 GET
/api/bar 0.06 GET
/api/bar 0.04 POST
/api/hoge 0.3 POST
/api/hoge 0.2 POST
/api/hoge 0.3 POST
~~~~

### sort by count

~~~~
$ ./uri-counter -f sample.log
Line of text: method, count, min, max, avg, uri

POST    1       0.040   0.040   0.040   /api/bar
GET     2       0.010   0.050   0.030   /api/foo
GET     2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
GET     3       0.060   0.300   0.147   /api/bar
POST    3       0.200   0.300   0.267   /api/hoge
POST    3       0.030   0.090   0.063   /api/foo
~~~~

### sort by min

~~~~
$  ./uri-counter -f sample.log --sort-min
Line of text: method, count, min, max, avg, uri

GET     2       0.010   0.050   0.030   /api/foo
POST    3       0.030   0.090   0.063   /api/foo
POST    1       0.040   0.040   0.040   /api/bar
GET     3       0.060   0.300   0.147   /api/bar
GET     2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
POST    3       0.200   0.300   0.267   /api/hoge
~~~~

### sort by max(reverse)

~~~~
$ ./uri-counter -f sample.log --sort-max -r
Line of text: method, count, min, max, avg, uri

GET     3       0.060   0.300   0.147   /api/bar
POST    3       0.200   0.300   0.267   /api/hoge
GET     2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
POST    3       0.030   0.090   0.063   /api/foo
GET     2       0.010   0.050   0.030   /api/foo
POST    1       0.040   0.040   0.040   /api/bar
~~~~

### sort by avg(reverse)

~~~~
$ ./uri-counter -f sample.log --sort-avg -r
Line of text: method, count, min, max, avg, uri

POST    3       0.200   0.300   0.267   /api/hoge
GET     2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
GET     3       0.060   0.300   0.147   /api/bar
POST    3       0.030   0.090   0.063   /api/foo
POST    1       0.040   0.040   0.040   /api/bar
GET     2       0.010   0.050   0.030   /api/foo
~~~~

## sample (count)

sample log file

~~~~
/api/hoge
/api/test?foo=bar
/api/test?foo=baar
/api/test?foo=baaar
/api/hoge
/api/foobar
~~~~

### sort by count

~~~~
$ ./uri-counter -f sample2.log -r
Line of text: method, count, uri

GET     3       /api/test?foo=xxx
GET     2       /api/hoge
GET     1       /api/foobar
~~~~