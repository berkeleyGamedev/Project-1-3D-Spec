## Section 4 - Attacks

In this section, we will be creating two abilties to damage the enemies we created in last section.

#### Summary:
1. We will be creating the scripts for the two abilities our player will be using, <ins>LaserAttack</ins> and <ins>MegaLaserAttack</ins> scripts.
2. Then, we will attach these scripts to the prefab objects of the same name.
3. Finally, we will be updating our <ins>PlayerController</ins> script so that the player is able to use the abilities and kill some enemies!

### Understanding Ability and AbilityInfo Scripts
Before we start writing the scripts for each our player abilities we need to understand what the <ins>Ability</ins> and <ins>AbilityInfo</ins> scripts do. Navigate to your Ability scripts folder `Assets > Scripts > Player > Abilities` to see these scripts.

<ins>AbilityInfo</ins>
We call this script a data structure, as it will hold important data for each of our abilites and getter functions to be able to retrieve the data from outside the script.

```
public class AbilityInfo
{
    #region Editor Variables
    [SerializeField]
    [Tooltip("How much power this ability has")]
    private int m_Power;

    public int Power
    {
        get
        {
            return m_Power;
        }
    }

    [SerializeField]
    private float m_Range;
    [Tooltip("If this is an attack that shoots something out, this value describes how ar the attack can shoot")]
    public float Range
    {
        get
        {
            return m_Range;
        }
    }   
    #endregion

}
```

- `m_Power` is the damage our ability inflicts on the enemy
- `Power` returns the m_Power of the ability
- `m_Range` is how far can our ability shoot out
- `Range` returns the m_Range


<ins>Ability</ins>
We call this script an abstract class, which creates functions that will be used by our ability subclasses. We also create protected variables that can only be accessed and used by subclasses of the <ins>Ability</ins> script.

```
public abstract class Ability : MonoBehaviour
{
    #region Editor Variables
    [SerializeField]
    [Tooltip("All of the main information about this particular ability.")]
    protected AbilityInfo m_info;
    #endregion

    #region Cached Components
    protected ParticleSystem cc_PS;
    #endregion

    #region Intialization
    private void Awake()
    {
        cc_PS = GetComponent<ParticleSystem>();
    }
    #endregion

    #region Use Methods
    public abstract void Use(Vector3 spawnPos);

    #endregion
}
```
- `m_info` This contains all the ability information for a specific ability subclass, the same information described in the <ins>AbilityInfo</ins>
- `cc_PS` This contains the particle system component for a specific ability that will make it visually appealing. (More on this later in this section!)
- `Awake` When the game is started the `cc_PS` variable will store the current particle system attatched to the ability.
- `Use` This method will trigger our abilties and takes in a Vector3 `spawnPos` that tells us the origin of where our ability should shoot out from. This method is abstract as we will be customizing it for each of our abilities in their own <ins>Ability</ins> subclass scripts.

### Implenting Laser Attack
Our first task will be implenting our first ability: laser attack. This attack will simply fire off a laser that will hit a single target. In the same `Abilities` folder, create a new script called <ins>LaserAttack</ins>. Notice that we want this script to be a subclass of <ins>Ability</ins> as such we need to change the top of the class to,
```
public class LaserAttack : Ability
```
making our script a subclass. 
{: .note} 
>Notice that this becomes highlighted in red due to us not having written the methods inherited from the <ins>Ability</ins> script. Just use ctrl + '.' to let Visual Studio implement it for you. Before continuning, remove the `Awake()` and `Update()` method and ensure your script looks like the one below.
'''
public class LaserAttack : ability
{
    public override void Use(Vector3 spawnPos)
    {
        
    }
}
'''

**Task 4.1: In `Use()`, create a Raycast object named `hit` and create an if condition statement to check for any collisions. If there is a collision, compare the tag of the hit object with that of "Enemy". If it is an enemy, then call the `DecreaseHealth()` method of the enemy **

{: .hint}
You will want to use `Physics.SphereCast` to create the hitbox of the laser. The `hit` variable will be holding the object that collides first with the laser and what you will be comparing tags to in the 'If' statement. [Check out the documentation to learn more!](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/Physics.SphereCast.html)

{: .hint}
To check for tags use the `CompareTag("Tag name")` method. [Check out the documentation to learn more!](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/Component.CompareTag.html)

{: .hint}
To get a specific script from a gameobject use the `GetComponent<"name of script">()` method. [Check out the documentation to learn more!](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/GameObject.GetComponent.html)

Lastly, please copy these lines of code to the end of the `Use()` method, they are what will create the visual look of our laser once we actually shoot it in the game.
```
var emitterShape = cc_PS.shape;
emitterShape.length = m_info.Range;
cc_PS.Play();
```
Nice, we have just implented our first ability! 

### Implementing Mega Laser Attack
Now, we will be creating our bigger more powerful attack that will shoot a large beam that can hit multiple enemies for a lot of damage! In the same folder as the previous section we will be creating the <ins>MegaLaserAttack</ins> script and also changing the top of the class like before.

```
public class MegaLaserAttack : ability
{
    public override void Use(Vector3 spawnPos)
    {
        
    }
}
```
Task 4.2: In `Use()`, similar to last task we will be creating our attacks however, this time we will be creating an array of Raycast objects, called `hits`. Then, iterate through `hits` to check if an enemy was hit through comparing tags with "Enemy" and then call the `DecreaseHealth()` to make them take damage. 

{: .hint}
The implementation here should be very similar to that of <ins>LaserAttack</ins> script. The biggest change would be creating an array of hits and iterating through them!

Again, please include these lines of code at the end of the method so that we can actually see our mega laser shoot in game.

```
var emitterShape = cc_PS.shape;
emitterShape.length = m_info.Range;
cc_PS.Play();
```
Good job, we have created both of our player attack abilities. Next, we will be actually implenting them inside the game and connecting it to our player.

### Attaching Scripts to Prefabs

In this part, we will be attatching the scripts we just made to their respective prefab objects.

First, navigate to `Assets > Prefabs` and click on the 'Laser' prefab. Then, attach your <ins>LaserAttack</ins> script to it. Also, attach a <ins>DestroyInXSeconds</ins> script. Please ensure that your hierarchy view of the 'Laser' prefab looks the same as the picture below.

![disable view](images/fig4.1.png)\
Fig 4.1


