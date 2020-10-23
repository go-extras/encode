Context-aware JSON encoder/decoder
==================================

This library is an experiment to add a context to the standard json encoding/decoding library.

## How to use

### Encoding

Your struct should implement the following interface:

```go
// ContextAwareMarshaler is the interface implemented by types that
// can marshal themselves into valid JSON. In addition, it provides
// an arbitrary context field that can be passed from MarshalWithContext.
type ContextAwareMarshaler interface {
	MarshalJSONWithContext(ctx interface{}) ([]byte, error)
}
```

To utilize the context, you should call `MarshalWithContext` instead of `Marshal`:

```go
encoded, err := MarshalWithContext(obj, "your context data")
```

### Decoding

Your struct should implement the following interface:

```go
// ContextAwareUnmarshaler is the interface implemented by types
// that can unmarshal a JSON description of themselves.
// The input can be assumed to be a valid encoding of
// a JSON value. UnmarshalJSONWithContext must copy the JSON data
// if it wishes to retain the data after returning. In addition,
// it can use an extra argument with the arbitrary context data
// that can be passed via UnmarshalWithContext.
//
// By convention, to approximate the behavior of Unmarshal itself,
// Unmarshalers implement UnmarshalJSON([]byte("null")) as a no-op.
type ContextAwareUnmarshaler interface {
	UnmarshalJSONWithContext([]byte, interface{}) error
}
```

To utilize the context, you should call `UnmarshalWithContext` instead of `Unmarshal`:

```go
err := UnmarshalWithContext(buf, &obj, "your context data")
```