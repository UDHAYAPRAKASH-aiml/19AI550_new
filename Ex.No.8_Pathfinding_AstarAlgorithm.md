# Ex.No: 8  Implementation of Path finding using A* algorithm
### DATE: 20/11/26                                                                  
### REGISTER NUMBER : 212224240177
### AIM: 
To write a program to create graph using waypoints and use A* algorithm to find path between source and destination.
### Algorithm:
```
1. Create a New Unity Project by Open the  Unity Hub and create a new 3D Project,Name the project (e.g., Pathfinding).
2. Create Waypoints in Scene => Create empty or sphere GameObjects ( minimum 4)  and  name it as Waypoint1, Waypoint2, ..., Waypoint4
   Position them freely in the scene (not on a grid)
3. Write a waypoint script to draw the lines between neighbors.
4.Attach waypoint.cs script to each waypoint GameObject and Manually assign neighbors in the Inspector to form a graph.
5. Create a empty game object and name it as WaypointManager - to manage all waypoints
6. Attach Waypoint script to it
7.Write a Pathfinding algorithm using A*search
8. Create a Game Object for Player ( choose capsule or any others) and attach the script to move player from start to end waypoints
```  
### Program:
```
**#1.Waypoint.cs**
            }

            openSet.Remove(current);

            foreach (var neighbor in current.neighbors) {
                float tentativeG = gScore[current] + Vector3.Distance(current.transform.position, neighbor.transform.position);
                if (tentativeG < gScore[neighbor]) {
                    cameFrom[neighbor] = current;
                    gScore[neighbor] = tentativeG;
                    fScore[neighbor] = tentativeG + Vector3.Distance(neighbor.transform.position, goal.transform.position);

                    if (!openSet.Contains(neighbor)) {
                        openSet.Add(neighbor);
                    }
                }
            }
        }

        return null; // No path found
    }

    private static List<Waypoint> ReconstructPath(Dictionary<Waypoint, Waypoint> cameFrom, Waypoint current) {
        var path = new List<Waypoint> { current };
        while (cameFrom.ContainsKey(current)) {
            current = cameFrom[current];
            path.Insert(0, current);
        }
        return path;
    }
}

**#4.AICharacter.cs**
using UnityEngine;
using System.Collections.Generic;

public class AICharacter : MonoBehaviour {
    public Waypoint startWaypoint;
    public Waypoint goalWaypoint;
    public float speed = 3f;

    private List<Waypoint> path;
    private int currentIndex = 0;

    void Start() {
        path = Pathfinding.FindPath(startWaypoint, goalWaypoint);
    }

    void Update() {
        if (path == null || currentIndex >= path.Count) return;

        Vector3 target = path[currentIndex].transform.position;
        transform.position = Vector3.MoveTowards(transform.position, target, speed * Time.deltaTime);

        if (Vector3.Distance(transform.position, target) < 0.1f) {
            currentIndex++;
        }
    }
}
Check the following
1. Waypoints placed in scene
2. Neighbors set manually via Inspector
3. WaypointGraph script on a manager
4. AICharacter assigned a start and goal
```
### Output:
<img width="1902" height="1077" alt="image" src="https://github.com/user-attachments/assets/1e7a23f5-b158-41c5-9fb3-02d2c5248c3c" />
<img width="987" height="625" alt="image" src="https://github.com/user-attachments/assets/c53bdaad-99b6-45c8-a40d-189d1aa9680c" />


### Result:
Thus the pathfinding algorithm was sucessfully implemented.

