# Toolbox

## Introduction
The "Toolbox" is a concept introduced by J.B. Rainsberger in the article Use your singletons wisely. The basic idea being that the application, not the components, should be the singleton. This basically means creating one singleton (the toolbox) that aggregates and manages all other classes that would normally be singletons themselves. This improves upon the concept by reducing coupling and making it much easier to test.

## Requirements
Uses Singleton base class.

## Usage
Any required components should be defined in the toolbox class itself. So for example, let's say we have a very important PlayerData MonoBehaviour. We should create an instance of this in the toolbox and add a public function to allow us to retrieve it.

----
private PlayerData m_PlayerData = new PlayerData();
 
public PlayerData GetPlayerData()
{
    return m_PlayerData;
}
----

Now to retrieve this global component we simply use Toolbox.Instance followed by the function we defined in the toolbox.

----
var playerData = Toolbox.Instance.GetPlayerData();
Debug.Log(playerData.CurrentLevel

----
You can also add, get or remove global components at runtime. However, these methods use strings for IDs which are error-prone, so use with cation!
----
// Add and get a global component.
var playerData = Toolbox.Instance.AddGlobalComponent("PlayerData", PlayerData);
Debug.Log(playerData.CurrentLevel);
 
// Getting a global component.
var playerData = Toolbox.Instance.GetGlobalComponent("PlayerData");
Debug.Log(playerData.CurrentLevel);
 
// Delete existing global component.
Toolbox.Instance.RemoveGlobalComponent("PlayerData");
----

Note: It's best not to add or delete components directly on the toolbox yourself, since this will likely lead to problems.

