# Dependency injection

<div style='color: red'>
PLUGIN USES REFLECTION AND THUS DOES NOT WORK WITH WEB TARGET YET
</div>


#### How to use
1. Implement [IDecembristConfiguration](https://github.com/decembrist-revolt/godot-decembrist-plugin/blob/master/addons/decembrist_plugin/Di/IDecembristConfiguration.cs) interface
2. Class name should be **DecembristConfiguration** (with empty namespace)
   > you can change config class name through [Config Class in plugin settings](/0.3-beta/settings.md?id=config-classstring)
   
   [Example](https://github.com/decembrist-revolt/godot-decembrist-plugin/blob/master/Example/DecembristConfiguration.cs)
   ```csharp
    namespace Appname.Appnamespace
    {
        public class Configuration : IDecembristConfiguration
        {
            public ContainerBuilder ConfigDi(ContainerBuilder builder)
            {
                builder.RegisterInstance(InstanceService.Instance);
                builder.Register<SingletonService3>();
                builder.Register<SingletonService2, IService>();
                builder.Register<SingletonService1>();
                builder.RegisterPrototype<PrototypeService1>();
                builder.RegisterPrototype<PrototypeService2>();
                return builder;
            }
        }
    }
   ```
   > In such case your Config Class settings should be = _Appname.Appnamespace.Configuration_
   
3. Register dependencies through [register methods](/0.3-beta/di.md?id=register-dependencies)
   ```csharp
   class Service2 
   {
        ...
   }
   
   class Service1
   {
        private readonly Service2 _service2;
   
        public Service1(Service2 service2)
        {
            _service2 = service2;
        }
        
        ...
   }
   
   public class DecembristConfiguration : IDecembristConfiguration
   {
        public ContainerBuilder ConfigDi(ContainerBuilder builder)
        {
            builder.Register<Service1>();
            builder.Register<Service2>();
            return builder;
        }
   }
   ```
4. Inject classes for your nodes through [inject attributes](/0.3-beta/di.md?id=injection-attributes)
   ```csharp
   class SomeScene : Node {
        [Inject]
        private Service1 _service1;
   }
   ```
5. Invoke this.InjectAll() at node's _Ready method
   ```csharp
   class SomeScene : Node {
   
        ...
        
        public override void _Ready()
        {
            this.InjectAll();
        }
        
   }
   ```
[Example](https://github.com/decembrist-revolt/godot-decembrist-plugin/tree/master/Example)

#### Dependency rules
Every dependency class will be resolved by first constructor (by reflection order)  
For [example](https://github.com/decembrist-revolt/godot-decembrist-plugin/blob/master/Example/Service/SingletonService2.cs)
in this case SingletonService1 will be injected from DI container while resolving step
#### Register dependencies
1. **Register\<Type>()**
```csharp
builder.Register<ServiceClass>();
```
Registered class will be instantiated as singleton(only one instance for every place).  
Such dependency could be injected as ServiceClass type
> Same as **Register\<Type, Type>()**

2. **Register\<Type, AsType>()**
```csharp
builder.Register<ServiceClass, IService>();
```
Registered class will be instantiated as singleton(only one instance for every place).  
Such dependency could be injected as IService type

3. **RegisterInstance(object)**
```csharp
builder.RegisterInstance<IService>(new ServiceClass());
```
Registered instance will be registered as singleton dependency.  
Such dependency could be injected as IService type
4. Prototype scope  
You can pass scope param to _Register_ methods
```csharp
builder.Register<ServiceClass>(DependencyScope.Prototype)
builder.Register<ServiceClass, IService>(DependencyScope.Prototype);
//Same as
builder.RegisterPrototype<ServiceClass>()
builder.RegisterPrototype<ServiceClass, IService>();
```
Dependencies with prototype scope will be instantiated for every inject place
#### Injection attributes
```csharp
[Inject]
private Service _service;
```
Inject dependency for class after this.InjectAll() call
Dependency type defined from field type
```csharp
[SettingsValue("settings/name")]
private string _settingValue;
```
Inject value from settings with "settings/name" name for class after this.InjectAll() call
Settings type defined from field type