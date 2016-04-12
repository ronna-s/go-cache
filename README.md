# go-cache
Go cache rails style

In RoR you can do this:

```ruby
Rails.cache.fetch "some_key" do
  "some value"
end
```

And in case of a cache miss, the return value of the block (i.e. "some value") will be written to cache.
Now let's do the same in go:

```go
cache.Fetch("some_key", func() interface{}{
  return "some_value"
})
```

TBD: signature - should probably let the store scan the result to the object


###Batch operations - pipeline

```go
cache.Do(func(pipe cache.Batch) error {
  pipe.Read("some_key1")
  pipe.Read("some_key2")
}
```

TBD: signature - batch operation can either maintain an array of inputs and outputs or use a channel internally.
