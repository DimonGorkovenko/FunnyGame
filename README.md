# FunnyGame
A short Unity project featuring sword combat, enemy AI (roam/chase states). Built with C# and SOLID principles.

# FunnyGame  
*Genre: Top-Down 2D Action*  
*Platform: Windows*  
*Engine: Unity*  

## Key Features  
- **Combat System**:  
  - Combo attacks with animations  
  - Attack stance  
  - Advanced enemy AI (roam, chase, attack, return to patrol point)  

- **Tools**:  
  - State Machine for enemy behavior  
  - NavMesh for enemy movement  

## Installation  
### Windows  
1. Download the folder  
2. Run `FunnyGame.exe`  

## Controls  
| Action | Key |  
|--------|-----|  
| Movement | WASD |  
| Attack stance | RMB |  
| Attack | LMB |  

## 📊 Skills Developed  
- **C#**:  
  - Implemented State Machine for enemy AI  
  - Worked with Unity UI  
  - Used interfaces  

- **Unity**:  
  - Configured 2D physics and collisions  
  - Set up 2D animations  
  - Implemented async operations  

- **Soft Skills**:  
  - SOLID architecture design  
  - Technical documentation writing  

## Code Highlights  

**Enemy State Controller**:  
```csharp
private void PhaseHandler()
{
    if (!_linkedToStartingPosition)
        _startingPosition = transform.position;

    switch (_currentPhase)
    {
        default:
        case Phase.Idle:
            CheckChasing();
            break;
        case Phase.Roaming:
            if (Vector2.Distance(transform.position, _positionToAchieve) < _minDistanceToStop)
            {
                _currentState = State.Idle;
                if (Time.time >= _nextRoamTime)
                {
                    _nextRoamTime = Time.time + _roamRate;
                    Roaming();
                }
            }
            if (PlayerRespawner.Instance.IsPlayerAlive)
                CheckChasing();
            break;
        case Phase.StartingToBeAgressive:
            CheckRoaming();
            ToChasing();
            break;
        case Phase.Chasing:
            if (Time.time > _nextChaseTime)
            {
                _nextChaseTime = Time.time + _chaseRate;
                Chasing();
                CheckRoaming();
                CheckAttacking();
            }
            break;
        case Phase.Attacking:
            Attacking();
            CheckRoaming();
            CheckChasing();
            break;
    }
}
```

**Asynchronous Enemy Spawning**:  
```csharp
void Start() => StartCoroutine(SpawnEnemies());

IEnumerator SpawnEnemies()
{
    while (true)
    {
        if (_currentEnemies < _maxEnemies && _spawnPoints.Length > 0)
        {
            Transform randomPoint = _spawnPoints[Random.Range(0, _spawnPoints.Length)];
            Instantiate(_enemyPrefab, randomPoint.position, Quaternion.identity);
            _currentEnemies++;
        }
        yield return new WaitForSeconds(_spawnInterval);
    }
}
```

**Health Bar Interface**:  
```csharp
public interface IHpContainable
{
    Transform transform { get; }
    float HpMax { get; }
    float HpCur { get; }
}
```
