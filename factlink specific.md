#Things that we need to discuss
##Source Code Layout
@jjoos likes the normal ident version, because in the last variant lines usually still end up being too long:
```ruby
# starting point (line is too long)
def send_mail(source)
  Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
end

# bad (normal indent)
def send_mail(source)
  Mailer.deliver(
    to: 'bob@example.com',
    from: 'us@example.com',
    subject: 'Important message',
    body: source.text)
end

# bad (double indent)
def send_mail(source)
  Mailer.deliver(
      to: 'bob@example.com',
      from: 'us@example.com',
      subject: 'Important message',
      body: source.text)
end

# good
def send_mail(source)
  Mailer.deliver(to: 'bob@example.com',
                 from: 'us@example.com',
                 subject: 'Important message',
                 body: source.text)
end
```

##Syntax
@markijbema prefers the first over the second(, only for defenition not invocation).
```ruby
 # bad
 def some_method_with_arguments arg1, arg2
   # body omitted
 end

 # good
 def some_method_with_arguments(arg1, arg2)
   # body omitted
 end
```

@jjoos doesn't like the last option.
```ruby
# bad
if some_condition
  do_something
end

# good
do_something if some_condition

# another good option
some_condition && do_something
```

```ruby
# bad
paths = [paths] unless paths.is_a? Array
paths.each { |path| do_something(path) }

# good
[*paths].each { |path| do_something(path) }

# good (and a bit more readable)
Array(paths).each { |path| do_something(path) }
```
@markijbema and @jjoos are wtf about the second. 
```ruby
# bad
paths = [paths] unless paths.is_a? Array
paths.each { |path| do_something(path) }

# good
[*paths].each { |path| do_something(path) }

# good (and a bit more readable)
Array(paths).each { |path| do_something(path) }
```
##Classes & Modules
We use identation here do we want to keep doing that?
```ruby
class SomeClass
  def public_method
    # ...
  end

  private

  def private_method
    # ...
  end

  def another_private_method
    # ...
  end
end
```
##Exceptions
Didn't know ```rescue Exception``` was that bad.
```ruby
# bad
begin
  # calls to exit and kill signals will be caught (except kill -9)
  exit
rescue Exception
  puts "you didn't really want to exit, right?"
  # exception handling
end
```
##Collections
@markijbema and @jjoos are no way about:
```ruby
# bad
STATES = ['draft', 'open', 'closed']

# good
STATES = %w(draft open closed)
```
and
```ruby
# bad
STATES = [:draft, :open, :closed]

# good
STATES = %i(draft open closed)
```
What do you guys think about this one? Seems like a good use case for a %r?
```ruby
regexp = %r{
  start         # some text
  \s            # white space char
  (group)       # first group
  (?:alt1|alt2) # some alternation
  end
}x
```
@jjoos doesn't like this too much, shouldn't have too much html in ruby.
```ruby
# bad (no interpolation needed)
%(<div class="text">Some text</div>)
# should be '<div class="text">Some text</div>'

# bad (no double-quotes)
%(This is #{quality} style)
# should be "This is #{quality} style"

# bad (multiple lines)
%(<div>\n<span class="big">#{exclamation}</span>\n</div>)
# should be a heredoc.

# good (requires interpolation, has quotes, single line)
%(<tr><td class="name">#{name}</td>)
```
What do you guys think about this?
```ruby
# bad
date = %x(date)

# good
date = `date`
echo = %x(echo `date`)
```
