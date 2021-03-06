# HBase For Ruby Made Easy

<!--[![Build Status](https://secure.travis-ci.org/highgroove/hbase-rb.png)](http://travis-ci.org/highgroove/hbase-rb)-->

To use [HBase](http://hbase.apache.org/) with [Apache
Thrift](http://thrift.apache.org/), you might have to manually generate
Ruby code from generic *.thrift files.

This gem tries to alleviate that slight annoyance by packaging everything
you need to communicate with HBase.

## Installation

### Bundler

Add to `Gemfile` and run `bundle install`:

```ruby
gem 'hbase-rb'
```

### Without Bundler

Install the gem:

```bash
gem install hbase-rb
```

Require it explicitly in your scripts:

```ruby
require "rubygems"
require "hbase-rb"
```

## Versioning

The version of the gem matches the version of HBase the interface was
generated against.

For instance, when using HBase 0.90.4:

```ruby
gem 'hbase-rb', '0.90.4'
```

or HBase 0.94.9:

```ruby
gem 'hbase-rb', '0.94.9'
```

## Usage

This library simply exposes the generated Ruby code from Thrift.
**Therefore it is not necessarily very idiomatic Ruby.**

For example:

```ruby
socket    = Thrift::Socket.new('localhost', 9090)

transport = Thrift::BufferedTransport.new(socket)
transport.open

protocol  = Thrift::BinaryProtocol.new(transport)
client    = HBase::Client.new(protocol)

puts client.getTableNames
```

## Writing Rows

```ruby
# Assuming a table "mytable" and a column family "c"
client.mutateRow("mytable", "abc123", [HBase::Mutation.new(column: "c:foo", value: "bar")])
```

## Reading Rows

```ruby
# Assuming a table "mytable" and a column family "c"
results = client.getRow("mytable", "abc123")
if result = results.first
  result.columns.each do |key, cell|
    puts "#{key}: #{cell.value}"
  end
end
```

## API Reference

* [Hbase/ThriftApi](http://wiki.apache.org/hadoop/Hbase/ThriftApi)
