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
	
results, err := cache.Do(func(batch cache.Batch) error {
  batch.Fetch("some_key1", func() ([]byte, error) { return "hello world", nil })
  batch.Get("some_key1")
  batch.Write("some_key2", 2*time.Second, []byte{"hello hello"})
  batch.Get("some_key2")
  return nil
})
if err == nil{
  //results is a channel that will closed the last response is read
  data1, err = results.Pop() //Fetch
  data2, err = results.Pop() //Get
  data3, err = results.Pop() //Write
  data3, err = results.Pop() //Get
}
```

