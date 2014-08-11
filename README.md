# CKAN performance testing

This repository contains a couple of simple shell scripts for running
performance and load tests against CKAN.

## requirements

The tests require the following testing tools to be installed:

- [wrk](https://github.com/wg/wrk)
- [vegeta](https://github.com/tsenart/vegeta)

## urls

The tests use an input file containing URL paths to test. Each URL is tested
individually by the performance testing script, while the load testing script
issues requests to randomly-chosen URLs from the input file.

An example input file is supplied in `views`, for use with a CKAN instance that
has had the [demo datasets](https://github.com/ckan/ckan-demo-data) loaded, but
**note that resource IDs are currently not fixed by the demo data and will need
to be updated in the `views` file before using these scripts**.

## performance tests

Run:

    ./perf http://ckanrooturl.com <views

A report will be generated with a filename of the form:

    YYYYMMDDHHMMSS_perf.txt

## load tests

Run:

    ./perf http://ckanrooturl.com <views

This will run tests at several request rates, and reports will be generated with
filenames of the form:

    YYYYMMDDHHMMSS_load_N.bin

`N` indicates the request rate for the test run. You can override the default
set of request rates by setting the `REQUEST_RATES` environment variable. For
example, to run load tests at 2, 8, and 16 req/s, run:

    REQUEST_RATES="2 8 16" ./perf http://ckanrooturl.com <views

## processing reports

The report generated by the performance tests is textual.

The reports generated by the load tests are binary reports from the `vegeta`
tool and can be parsed into a textual summary using the `vegeta report` command:

    vegeta report <20140811T111729_load_10.bin

You can also generate HTML plot reports:

    vegeta report -reporter plot >out.html <20140811T111729_load_10.bin

## caveats

DO NOT run performance or load tests against CKAN instances you do not control.

It is probably unwise to run performance or load tests against public CKAN
instances (or public websites in general).

The results of these tests reflect the behaviour of CKAN under highly artificial
loading conditions and you should be cautious when attempting to forecast CKAN's
performance under real loading conditions on the basis of the output of these
tools.