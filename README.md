# Toolbox

## Introduction
The "Toolbox" is a concept introduced by J.B. Rainsberger in the article Use your singletons wisely. The basic idea being that the application, not the components, should be the singleton. This basically means creating one singleton (the toolbox) that aggregates and manages all other classes that would normally be singletons themselves. This improves upon the concept by reducing coupling and making it much easier to test.

## Requirements
Uses Singleton base class.

## Usage
Any required components should be defined in the toolbox class itself. So for example, let's say we have a very important PlayerData MonoBehaviour. We should create an instance of this in the toolbox and add a public function to allow us to retrieve it.

```
private PlayerData m_PlayerData = new PlayerData();
 
public PlayerData GetPlayerData()
{
    return m_PlayerData;
}
```

Now to retrieve this global component we simply use Toolbox.Instance followed by the function we defined in the toolbox.

```
var playerData = Toolbox.Instance.GetPlayerData();
Debug.Log(playerData.CurrentLevel

```
You can also add, get or remove global components at runtime. However, these methods use strings for IDs which are error-prone, so use with cation!
```
// Add and get a global component.
var playerData = Toolbox.Instance.AddGlobalComponent("PlayerData", PlayerData);
Debug.Log(playerData.CurrentLevel);
 
// Getting a global component.
var playerData = Toolbox.Instance.GetGlobalComponent("PlayerData");
Debug.Log(playerData.CurrentLevel);
 
// Delete existing global component.
Toolbox.Instance.RemoveGlobalComponent("PlayerData");
```

Note: It's best not to add or delete components directly on the toolbox yourself, since this will likely lead to problems.



# Singleton

## Alternatives
Scriptable Objects
One excellent alternative to the singleton pattern in Unity is the use of ScriptableObjects as a type of global variable. Ryan Hipple from Schell Games gave a presentation at Unite Austin 2017 titled Game Architecture with Scriptable Objects that explains how to implement them and the many advantages over singletons.

## Introduction
The singleton pattern is a way to ensure a class has only a single globally accessible instance available at all times. Behaving much like a regular static class but with some advantages. This is very useful for making global manager type classes that hold global variables and functions that many other classes need to access. However, the convenience of the pattern can easily lead to misuse and abuse and this has made it somewhat controversial with many critics considered it an anti-pattern that should be avoided. But like any design pattern, singletons can be very useful in certain situations and it's ultimately up to the developer to decide if it's right for them.

## Advantages
Globally accessible. No need to search for or maintain a reference to the class.
Persistent data. Can be used to maintain data across scenes.
Supports interfaces. Static classes can not implement interfaces.
Supports inheritance. Static classes can not inherent for another class.
The advantage of using singletons in Unity, rather than static parameters and methods, is that static classes are lazy-loaded when they are first referenced, but must have an empty static constructor (or one is generated for you). This means it's easier to mess up and break code if you're not careful and know what you're doing. As for using the Singleton Pattern, you automatically already do lots of neat stuff, such as creating them with a static initialization method and making them immutable.

## Disadvantages
Must use the Instance keyword (e.g. <ClassName>.Instance) to access the singleton class.
There can only ever be one instance of the class active at a time.
Tight connections. Modifying the singleton can easily break all other code that depends on it. Requiring a lot of refactoring.
No polymorphism.
Not very testable.
Implementation
The singleton pattern is commonly applied to multiple classes but the implementation is always the same. So creating a singleton base class others can inherit from is ideal because it eliminates the need to recreate the same code over and over again for each class that needs to be a singleton.

## Notes:
This script will not prevent non singleton constructors from being used in your derived classes. To prevent this, add a protected constructor to each derived class.
When Unity quits it destroys objects in a random order and this can create issues for singletons. So we prevent access to the singleton instance when the application quits to prevent problems.

## Requirements
The above script makes use of the custom extension method GetOrAddComponent which is not a part of Unity.

## Usage
To make any class a singleton, simply inherit from the Singleton base class instead of MonoBehaviour, like so:
```
public class MySingleton : Singleton<MySingleton>
{
    // (Optional) Prevent non-singleton constructor use.
    protected MySingleton() { }
 
    // Then add whatever code to the class you need as you normally would.
    public string MyTestString = "Hello world!";
}
```
Now you can access all public fields, properties and methods from the class anywhere using <ClassName>.Instance:
```
public class MyClass : MonoBehaviour
{
    private void OnEnable()
    {
        Debug.Log(MySingleton.Instance.MyTestString);
    }
}
```
