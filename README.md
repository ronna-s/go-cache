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

or, in case your function can fail and you want to avoid writing to the cache store

```go
cache.FetchDiscreetly("some_key", func() (interface{}, error){
  return "some_value", nil
})
```
