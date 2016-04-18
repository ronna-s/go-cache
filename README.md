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
	data, err = cache.Fetch("some_key", func() ([]byte, error) {
		return []byte{"some_value"}, nil
	})
```

TBD: signature - should probably let the store scan the result to an object


###Batch operations - pipeline

```go

	cache.Do(func(batch cache.Batch) error {
		data, _ := batch.Fetch("some_key1", func() ([]byte, error) { return "hello world", nil })
		fmt.Println(string(data)) //hello world
		data, _ = batch.Get("some_key1")
		fmt.Println(string(data)) //hello world
		data = []byte{"hello hello"}
		batch.Write("some_key2", 2*time.Second, data)
		data, _ = batch.Get("some_key2")
		fmt.Println(string(data)) //hello hello
		return nil
	})
```

