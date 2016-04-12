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
	var (
		data []byte
		err  error
	)
	cache.Fetch("some_key", func() ([]byte, error) {
		return []byte{"some_value"}, nil
	})
```

TBD: signature - should probably let the store scan the result to an object


###Batch operations - pipeline

```go
	var (
		dataCh <-chan []byte
		err    error
	)
	dataCh, err = cache.Do(func(pipe cache.Batch) {
		pipe.Fetch("some_key1", func() ([]byte, error) { return "hello world", nil })
		pipe.Write("some_key2", 2*time.Second, []byte{"hello hello"})
		pipe.Fetch("some_key2")
	})
```

TBD: signature - batch operation probably suggests response to return over a channel.
