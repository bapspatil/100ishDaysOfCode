
# Lifecycle

Read this:

[Saved State module for ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate)

This lets handle system-initiated process death with your ViewModel.

```
val vm = ViewModelProvider(this, SavedStateVMFactory(this))
        .get(SavedStateViewModel::class.java)
```

Your ViewModel:
```
class SavedStateViewModel(private val state: SavedStateHandle) : ViewModel() { ... }
```

## Storing and retreiving values

The `SavedStateHandle` class has the methods you expect for a key-value map:

* get(String key)
* contains(String key)
* remove(String key)
* set(String key, T value)
* keys()
