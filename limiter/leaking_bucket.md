## How it works

**This image is good enough to describe:**

![leaking_bucket](images/leaking_bucket.png)


## Code
```ruby
class LeakingBucket
  def initialize(capacity, leak_rate)
    @capacity = capacity
    @buckets = []
    @last_leak_time = Time.now.to_f
    @leak_rate = leak_rate
  end

  def add
    leak if @buckets.size >= @capacity
    @buckets << Time.now.to_f
  end

  def can_process?
    leak
    @buckets.size < @capacity
  end

  private

  def leak
    current_time = Time.now.to_f
    time_elasped = current_time - @last_leak_time
    remove_requests = (time_elasped * @leak_rate).to_i
    @buckets.shift(remove_requests)
    @last_leak_time = current_time
  end
end

bucket = LeakingBucket.new(10, 2)

10.times { bucket.add }

if bucket.can_process?
  puts "Processing the request"
else
  puts "Request rate exceeded"
end
```
