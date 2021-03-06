---
title: Shell
description: Run shell commands from Ruby scripts
---

See [mentalized.net/journal/2010/03/08/5-ways-to-run-commands-from-ruby](https://mentalized.net/journal/2010/03/08/5-ways-to-run-commands-from-ruby/)


## Using backticks

This is the easiest way to run shell commands in Ruby.

```ruby
`COMMAND`
```

e.g.

```ruby
files = `ls`
# => "404.html\nCONTRIBUTING.md\nGemfile\nGemfile.lock"

puts files
# 404.html
# CONTRIBUTING.md
# Gemfile
# Gemfile.lock

files = split("\x0")
# ["404.html", "CONTRIBUTING.md", "Gemfile", "Gemfile.lock" ]
```

It is the lightest in syntax, compared with other programming languages. Not even any imports needed.


## Use popen3

From [gist](https://gist.github.com/zparnold/0e72d7d3563da2704b900e3b953a8229).

```ruby
require 'open3'

def run_command(cmd_text)
  Open3.popen3(cmd_text) do |stdin, stdout, stderr, wait_thr|
    exit_status = wait_thr.value
    if exit_status != 0
      puts stderr.read
      abort
    else
      puts stdout.read
    end
  end
end
```

From [article](https://redpanthers.co/different-ways-to-run-shell-commands-in-ruby/).

```ruby
require 'open3'

cmd = 'git push'
Open3.popen3(cmd) do |stdin, stdout, stderr, wait_thr|
  puts "stdout is:" + stdout.read
  puts "stderr is:" + stderr.read
end
```
