# Builder

```
type Builder struct {
	categoryExpanderFn CategoryExpanderFunc

	// mapper is set explicitly by resource builders
	mapper *mapper

	// clientConfigFn is a function to produce a client, *if* you need one
	clientConfigFn ClientConfigFunc

	restMapperFn RESTMapperFunc

	// objectTyper is statically determinant per-command invocation based on your internal or unstructured choice
	// it does not ever need to rely upon discovery.
	objectTyper runtime.ObjectTyper

	// codecFactory describes which codecs you want to use
	negotiatedSerializer runtime.NegotiatedSerializer

	// local indicates that we cannot make server calls
	local bool

	errs []error

	paths      []Visitor
	stream     bool
	stdinInUse bool
	dir        bool

	labelSelector     *string
	fieldSelector     *string
	selectAll         bool
	limitChunks       int64
	requestTransforms []RequestTransform

	resources   []string
	subresource string

	namespace    string
	allNamespace bool
	names        []string

	resourceTuples []resourceTuple

	defaultNamespace bool
	requireNamespace bool

	flatten bool
	latest  bool

	requireObject bool

	singleResourceType bool
	continueOnError    bool

	singleItemImplied bool

	schema ContentValidator

	// fakeClientFn is used for testing
	fakeClientFn FakeClientFunc
}
```

 ## Q&A

### 给 Builder 的成员变量赋值给 Builder 内另外的成员变量作用

```
# 待解决
```

