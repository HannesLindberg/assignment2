## New design

![New design UML diagram](<assignment2-new-design.drawio (3).png>)

### Explanation

| Relation | Type | Reason |
|-|-|-|
| `World` -> `Car` | has | holds the list of cars|
| `World` -> `WorldObserver` | has | holds the list of observers to notify|
| `DrawPanel` -> `WorldObserver` | implements | gets notified when the world changes|
| `DrawPanel` -> `World` | has | asks World for the cars when repainting|
| `CarController` -> `World` | has | sends button commands to World|
| `TimerListener` -> `World` | has | calls tick() every timer step|
| `CarView` -> `CarController` | has | the buttons call the controller|
| `CarView` -> `DrawPanel` | has | the window contains the drawing panel|
| `DrawPanel` -> `ImageLoader` | uses | uses it to get car images|
| `ImageLoader` -> `Car` | uses | picks an image based on the car|
| `Volvo240`, `Saab95` -> `Car` | extends | they are cars and share its code|
| `Car` -> `Movable` | implements | cars can move|

### Motivation

| Change | Problem it solves (from Task 3) | Principle | Why it's better |
|-|-|-|-|
| `World` holds the cars | In task 3, CarController had many responsibilities. The timer, user commands, moving the cars and repainting the view| SRP: a class should only have one reason to change| Now CarController only handles user input and the cars live in the model |
| `World.tick()`, timer outside | The timer is a Swing class, and Swing is part of the GUI| The model should not depend on the GUI| World can be tested without opening a window|
| Observer: `WorldObserver` | CarController and CarView depended on each other which is a mutual dependency| DIP: World depends on the WorldObserver interface not on a concrete view class| World does not know that DrawPanel exists and new views can be added without changing world|
| `DrawPanel` asks `World` + `ImageLoader` | DrawPanel loaded images, stored positions and painted so it had three jobs| SRP: each class has one job, and positions are only stored in the model| DrawPanel can now draw any number of cars|
