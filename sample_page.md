Sample Page can be edited here in the file sample_page.md
you can change whatever you want in here add some code as well if you want

Source code from project
```java
package unsw.dungeon.goals;

import java.util.ArrayList;
import java.util.List;

import org.json.JSONArray;
import org.json.JSONObject;

import javafx.beans.property.IntegerProperty;
import unsw.dungeon.observer.Observer;
import unsw.dungeon.observer.Subject;

/**
 * Goal controller initiated in goalLoader and is set in dungeon class
 * Is what loads the goals and also observers the goals below it and determines if the player has won the game
 * @author z5209733
 *
 */
public class GoalTreeController implements Observer, Subject{
	protected Goal root;
	List<Observer> observers = new ArrayList<>();
	
	/**
	 * Shows all goals under root inclusive
	 */
	public void showGoal() {
		root.showGoal();
	}
	
	/**
	 * Return string of goals
	 */
	public String textGoal() {
		return root.textGoal();
	}
	
	/**
	 * Checks if all goals are complete
	 * 
	 * @return boolean
	 * 		if all goals are complete 
	 * 			return true
	 *		else 
	 *			return false
	 */
	public boolean checkIfGoalsComplete() {
		return root.isComplete();
	}
	
	/**
	 * Called in dungeon loader to load goals
	 * @param json	the json object that will be parsed and added
	 */
	public void loadGoals(JSONObject json) {
		this.root = loadGoalsRecursive(json);
	}
	
	/**
     * Parse the goal-condition object to create Goal composite object
     * Recursive function that loops through AND and OR goals
     * 
     * @param json	the json object that will be parsed and added
     * @return Goal Object
     * 		returns the root Goal
     */
    private Goal loadGoalsRecursive(JSONObject json) {
    	String name = json.getString("goal");
    	CompositeGoal cGoal = null;
    	
    	//AND Branch, create a AndComposite goal and recursively loadGoals within it
    	if (name.equals("AND")) {
    		cGoal = new AndCompositeGoal();
    		JSONArray jsonSubgoals = json.getJSONArray("subgoals");
    		for (int i = 0; i < jsonSubgoals.length(); i++) {
    			 cGoal.add(loadGoalsRecursive(jsonSubgoals.getJSONObject(i)));    			
    		}
    		return cGoal;
    	} 
    	//OR Branch, create a OrComposite goal and recursively loadGoals within it
    	else if (name.equals("OR")) {
    		cGoal = new OrCompositeGoal();
    		JSONArray jsonSubgoals = json.getJSONArray("subgoals");
    		for (int i = 0; i < jsonSubgoals.length(); i++) {
    			 cGoal.add(loadGoalsRecursive(jsonSubgoals.getJSONObject(i)));    			
    		}
    		return cGoal;
    	} 
    	//Base Case
    	else {
    		Goal newGoal;
    		switch(name) {
    		case "exit":
    			newGoal = new ExitGoal();
    			break;
    		case "boulders":
    			newGoal = new BoulderGoal();
    			break;
    		case "treasure":
    			newGoal = new TreasureGoal();
    			break;
    		case "enemies":
    			newGoal = new EnemyGoal();
    			break;
    		default:
    			System.err.println("Invalid goal type was requested: " + name);
    			newGoal = null;
    		}
    		newGoal.attachObserver(this);
    		return newGoal;
    	}
    }
    
    public Goal getRoot() {
    	return root;
    }
    
    public IntegerProperty getGoalCount(String GoalType) {
    	List<Goal> searchGoal = getRoot().searchTree(GoalType);
    	if (searchGoal.size() == 0) {
    		return null;
    	}
    	return searchGoal.get(0).getCount();
    } 

    /**
     * Check all the isComplete() in it List<Goal>
     * Currently only return a console message, will change in milestone 3 and add JFX
     */
	@Override
	public void update(Subject obj) {
		if (root.isComplete()) {
			//somehow alert a higher being
			notifyObservers();
			System.out.println("\n=========All Goals Have Been Completed=========\n");
		}
	}

	@Override
	public void attachObserver(Observer o) {
		observers.add(o);
	}

	@Override
	public void removeObserver(Observer o) {
		observers.remove(o);
	}

	@Override
	public void notifyObservers() {
		for (Observer o : observers) {
			o.update(this);
		}
	}
}


```


**Project description:** Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### 1. Suggest hypotheses about the causes of observed phenomena

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

```javascript
if (isAwesome){
  return true
}
```

### 2. Assess assumptions on which statistical inference will be based

```javascript
if (isAwesome){
  return true
}
```

### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
