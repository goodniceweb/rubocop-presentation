class: center, middle

<div class="first-page__header">
  <img src="images/rubo-logo-horizontal.png" class="first-page__logo">
  <p class="first-page__caption">Based on<br/>Ruby Style Guide</p>
</div>

<div class="first-page__footer">
  <img src="images/sweets.png" class="first-page__footer-image">
</div>

# RuboCop - unify and sweetify

---
layout: true
class: center

<div class="regular-page__background">
  <img src="images/rubo-logo-horizontal.png" class="regular-page__logo">
</div>

---

# Agenda

---

# Agenda

1. What is RuboCop? Why Should We Care?

---

# Agenda

1. What is RuboCop? Why Should We Care?
2. Configuration & Extensions

---

# Agenda

1. What is RuboCop? Why Should We Care?
2. Configuration & Extensions
3. Real World Examples

---

# What is RuboCop?

---

# What is RuboCop?

RuboCop is a Ruby static code analyzer

and code formatter.

---

# Why Should We Care?

![Thinking face](images/thinking-face.png)

---

# Benefits It Gives

---

# Benefits It Gives

1. Make your code more readable

---

# Benefits It Gives

1. Make your code more readable
2. Help you to know make stupid mistakes

---

# Benefits It Gives

1. Make your code more readable
2. Help you to know make stupid mistakes
3. Reduce load from PR reviewers

---

# Benefits It Gives

1. Make your code more readable
2. Help you to know make stupid mistakes
3. Reduce load from PR reviewers
4. Make code style consistent across a whole company

---

# Code Example #1


```ruby
class QuoteProcessor
  PRICE_CONFIG = [{ foo: 'Total price:' }]
  def process_it_the_best_possible_way_to_make_all_customers_happy_and_give_us_all_their_money(quote, submission)
    @config = {
      "key1" => get_insurance(quote),
      'key2' => fetch_insurance_price(quote),
      :key3 => grab_document_policy(quote),
      key4: collect_limits(quote),
    }.symbolize_keys
    if !@config[:key2]&.process&.map(&:to_i).reduce(&:+).present?
      ""
    else
      "#{PRICE_CONFIG[:foo].to_s} #{@config[:key2]&.process&.map(&:to_i).reduce(&:+)}"
    end
  end
end
```

???

- long method names
- inconsistent keys in a hash
- inconsistent naming for mthods with the same purpose
- not used variables
- spaces, spaces everywhere
- code complexity
- redundant string creation when we have interpolation
- frozen consts

---

# Code Example #1 but better


```ruby
class QuoteProcessor
  PRICE_CONFIG = [{ foo: "Total price:" }].freeze

  def call(quote, _submission)
    @config = {
      key1: insurance(quote),
      key2: insurance_price(quote),
      key3: document_policy(quote),
      key4: limits(quote),
    }
    format_price(@config)
  end

  private

  def format_price(config)
    total = @config[:key2]&.process&.map(&:to_i)&.reduce(&:+)
    return '' if total.blank?

    "#{PRICE_CONFIG[:foo]} #{total}"
  end
end
```

???

- long method names
- inconsistent keys in a hash
- inconsistent naming for mthods with the same purpose
- not used variables
- spaces, spaces everywhere
- code complexity
- redundant string creation when we have interpolation
- frozen consts

---

# Configuration

---

# Configuration

```yaml
AllCops:
  TargetRubyVersion: 2.5
Documentation:
  Enabled: false
Metrics/LineLength:
  Max: 120
Metrics/BlockLength:
  Exclude:
    - 'spec/**/*'
```

---

# Extensions

---

# Extensions

```ruby
group :development, :test do
  gem 'rubocop'
  gem 'rubocop-performance'
  gem 'rubocop-rails'
  gem 'rubocop-rspec'
end
```

---

# Extensions

```yaml
require:
  - rubocop-performance
  - rubocop-rails
  - rubocop-rspec
```
---

# Real World Examples

---

# Real World Examples

```ruby
# bad
quote.update_attributes(attribute1: 105001000000, attribute2: 105001000000, attribute3: 105001000000, attribute4: 10500100000, attribute5: 105001000000)
```

???

Let's say you need to find a bug here? Do you happy about that?

---

# Real World Examples

```ruby
# bad
quote.update_attributes(attribute1: 105001000000, attribute2: 105001000000, attribute3: 105001000000, attribute4: 10500100000, attribute5: 105001000000)

# better
quote.update(
  attribute1: 105_001_000_000,
  attribute2: 105_001_000_000,
  attribute3: 105_001_000_000,
  attribute4: 10_500_100_000,
  attribute5: 105_001_000_000
)
```

???

With style guide it's very easy to find it out.

---

# Real World Examples

```ruby
# bad
class Processor
  class ProcessorError < Exception
  end

  UUIDS = { ... }
  def self.call(record_uuid)
    if record_uuid = UUIDS[:some_key]
      update_naming(record_uuid)
    else
      raise ProcessorError, "Issue for #{record_uuid}"
    end
  end

  private

  def self.update_naming(record_uuid)
    # Some code here
  end
end
```

???

- Exception class => StandardError
- styles there
- private class method
- accidential variable assignment in `if` statement

---

# Real World Examples

```ruby
# better
class Processor
  class ProcessorError < StandardError; end

  UUIDS = { ... }.freeze

  def self.call(record_uuid)
    return update_naming(record_uuid) if record_uuid == UUIDS[:some_key]

    raise ProcessorError, "Issue for #{record_uuid}"
  end

  private_class_method def self.update_naming(record_uuid)
    # Some code here
  end
end
```

??? 

next slide to show assignment

---

# Real World Examples

```ruby
# better
class Processor
  class ProcessorError < StandardError; end

  UUIDS = { ... }.freeze

  def self.call(record_uuid)
    return update_naming(record_uuid) if (record_uuid = UUIDS[:some_key])

    raise ProcessorError, "Issue for #{record_uuid}"
  end

  private_class_method def self.update_naming(record_uuid)
    # Some code here
  end
end
```

---

# Let's Recap

---

# Let's Recap

1. Make your code more readable

---

# Let's Recap

1. Make your code more readable
2. Help you to know make stupid mistakes

---

# Let's Recap

1. Make your code more readable
2. Help you to know make stupid mistakes
3. Reduce load from PR reviewers

---

# Let's Recap

1. Make your code more readable
2. Help you to know make stupid mistakes
3. Reduce load from PR reviewers
4. Make code style consistent across a whole company

---

# Are you convinced?

---

# Are you convinced?

```
inherit_gem:
  coverwallet_style: default.yml

AllCops:
  TargetRubyVersion: 2.5
```

---

# Thanks for the listening

---

# Questions











