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
Usage: request-counter -f FILE [option]

Options:
  -h, --help            show this help message and exit
  -f FILE, --file=FILE  path to log file
  -r, --reverse         reverse the result of comparisons
  --sort-min            sort by min
  --sort-max            sort by max
  --sort-avg            sort by avg
  --no-headers          print no header line at all
~~~~

## sample (count and min, max, avg)

sample log file

~~~~
/api/test?foo=bar&ho=ge 0.1
/api/test?ho=geee&foo=baar 0.2
/api/foo 0.01
/api/foo 0.03
/api/foo 0.05
/api/foo 0.07
/api/foo 0.09
/api/bar 0.08
/api/bar 0.3
/api/bar 0.06
/api/bar 0.04
/api/hoge 0.3
/api/hoge 0.2
/api/hoge 0.3
~~~~

### sort by count

~~~~
$ ./request-counter -f sample.log
Line of text: count, min, max, avg, uri

2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
3       0.200   0.300   0.267   /api/hoge
4       0.040   0.300   0.120   /api/bar
5       0.010   0.090   0.050   /api/foo
~~~~

### sort by min

~~~~
$ ./request-counter -f sample.log --sort-min
Line of text: count, min, max, avg, uri

5       0.010   0.090   0.050   /api/foo
4       0.040   0.300   0.120   /api/bar
2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
3       0.200   0.300   0.267   /api/hoge
~~~~

### sort by max(reverse)

~~~~
$ ./request-counter -f sample.log --sort-max -r
Line of text: count, min, max, avg, uri

3       0.200   0.300   0.267   /api/hoge
4       0.040   0.300   0.120   /api/bar
2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
5       0.010   0.090   0.050   /api/foo
~~~~

### sort by avg(reverse)

~~~~
$ ./request-counter -f sample.log --sort-avg -r
Line of text: count, min, max, avg, uri

3       0.200   0.300   0.267   /api/hoge
2       0.100   0.200   0.150   /api/test?foo=xxx&ho=xxx
4       0.040   0.300   0.120   /api/bar
5       0.010   0.090   0.050   /api/foo
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
Line of text: count, uri

3       /api/test?foo=xxx
2       /api/hoge
1       /api/foobar
~~~~