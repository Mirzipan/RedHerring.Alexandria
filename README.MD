# Red Herring Alexandria

![Nuget](https://img.shields.io/nuget/v/RedHerring.Alexandria)

A library of basic utilities.

## BitMask

Multiple variants of a BitMask, namely 8-bit, 16-bit, and 32-bit versions.
Usable for most of your bit masking and flagging needs.

## Collections

### Collection Dictionary

If you are tired of always having to manage collections when using a dictionary with collections as values, then this might be for you.
There are multiple variants, all of which use some form of pooling for the collections.

Available variants:
* **HashSetDictionary** - dictionary with HashSet values, accepts custom value comparers
* **ListDictionary** - dictionary with List values
* **MultiDictionary** - dictionary with Dictionary values, accepts custom inner key comparers

If none of the variants cover your needs, you may always just inherit from `CollectionDictionary` and make it use your custom `ICollection<T>`.

## IDisposerContainer

Interface that includes a `CompositeDisposable` property.
There is also an extension method `DisposeWith` for easier disposable registration.

```csharp
    public class Container : IDisposerContainer
    {
        private class Disposable : IDisposable
        {
            public void Dispose()
            {
                // TODO: cleanup
            }
        }
        
        private CompositeDisposable _disposer;

        CompositeDisposable IDisposerContainer.Disposer
        {
            get => _disposer ??= new CompositeDisposable();
            set => _disposer = value;
        }

        public IDisposable CreateDisposable()
        {
            return new Disposable().DisposeWith(this);
        }
    }
```

## Identifiers

Simple identifiers that combine the performance of an integer type with the convenience of a string.

Available variants:
* **QuadByte** - an identifier backed by `uint`
* **OctoByte** - an identifier backed by `ulong`
* **CompositeId** - an identifier backed by `ulong` with a primary and a secondary `QuadByte`

## Modules

Interfaces and abstract classes to make creating modules are little simpler and more consistent.
Modules need to implement `IModule` and module containers need to implement `IModuleContainer`.
Ideally, you would want to make your modules inherit from either `AModule` or `AModuleBehaviour`, depending on whether you are looking to implement a pure C# class or a `MonoBehaviour`.

```csharp
    public class SomeContainer : IModuleContainer
    {
        private readonly Dictionary<Type, SomeModule> _modules = new();

        public IModule Get(Type moduleType) => _modules.TryGetValue(moduleType, out var module) ? module : null;
    }

    public abstract class SomeModule : AModule<SomeContainer>
    {
        
    }
```
