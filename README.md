# iOS-Dev-Notes
Notes from the Flatiron School iOS Development Track (0216)

## Schedule

### Before Program
- Pre-work
- Mixer
- Campus is open
- Tours
- Skype interviews

### Week 1
- Three days of icebreakers
- Lectures about whys of program and background of instructors
- Review of student handbook and code of conduct
- Deploy on Day One - helps to overcome Imposter Syndrome
  - Do the minimum required to achieve the title you seek
  - Don't wait for everything to be perfect and prepared-Just Do It
- Solving problems with code
- Basic Objects
- Inheritance

### Week 2
- Basics Views and View Controllers
- Actions and Outlets
- Survey on program as a whole, admissions, pre-work, instructors
- Peer review every Friday here on out

### Week 3
- Navigation Controllers
- Segues
- Table Views

### Week 4
- Auto Layout & Constraints
  - Programatically
  - Interface Builder
- Stacks
- Data Stores & Singleton
- Communication Workshop
- 1-on-1s

### Week 5
- Core Data
- What's needed for job prep and getting hired
  - Knowledge
  - Comfort and confidence
  - Have a portfolio of blog posts and several good, small projects
- Core Data Relationships
- Delegates & Protocols
- Blocks

### Week 6
- The Internet & APIs
- GET Requests
- POST Requests
- Cocoapods

### Week 7
- OAuth
- Animations
- Gestures and Touches
- Xibs and custom views

### Week 8
- Notification center
- Obfuscating screen when leaving app
- Swift
- Optional Types in Swift
- Objects and Classes in Swift
- Blocks in Swift

### Weeks 9 - 12
- Project Mode
- Custom Delegates
- UI Basics
- GitHub
- Testing in Xcode
- NSError
- Technical Interviews
- Memory Management
- CSBS

## Minimum Viable Product
- Also known as "Minimum Desirable Product"
- Should grow from a simple solution to a simple problem into something more complex
- Should not grow from a partial solution to any problem
- Break things down into the smallest possible pieces
- Reduce everything into previously solved problems

## Common Errors
- "unrecognized selector sent to instance"
  - a method was called that doesn't exist
- look at the elements involved and consider what actions were taken on them
  - were they the wrong actions or the wrong objects/object types?
- use the Exception Breakpoint to trace crashes (in the breakpoint navigator)

## Classes
- puts data and functionality into same place
- allows addition of methods to objects
- a class is a blueprint for an object
- in a header file
```objc
#import <UIKit/UIKit.h>
@interface ClassItem : NSObject
@property (nonatomic, strong) NSString \*name; // For objects
@property (assign, nonatomic) NSUInteger price; // For primitives
- (void)nameOfAMethod:(NSString \*)name; // Adds a method which can be called on 'self'
@end
```
- almost never import an implementation file
- class methods can be called from other places, but can't be called from within an instance of the class's object

### Initializer Methods
- there can be helper methods called from an initializer
- ivars are set only in the class where the corresponding key is declared
- if you need to set an ivar from a subclass then ideally you should have an init which allows you to set the necessary vars
- if you have an additional argument to pass for initialization, the subclass initializer uses `self = [super initWith...]` to set the vars in the superclass and the new argument is set in the subclass initializer (see FISAmazingItem.m)

#### designated
- takes the most arguments and does the most work
- there can be multiple designated initializers

#### convenience
- calls on the designated initializer with arguments to set
- there can be multiple convenience initializers

#### default
- takes no arguments but sets default values (e.g., nil, empty strings, alloc init)

### Properties
- a property is data for a class
- For properties that are objects, be sure to initialize before use
- making a property creates three things
  - a getter method: retrieves the value of the property
  - a setter method: sets the value of the property
  - an ivar: an instance variable that _is_ the property's content
- properties can have attributes
  - nonatomic: prevents the setter and getter methods from running simultaneously on multiple threads (not such a big deal anymore, but still good practice to implement for most objects)
  - strong: retains the property in memory as long as the parent object is alive
  - weak: allows the property to leave memory when the parent is no longer in use
  - copy: creates a copy of the object to which it is set before calling the setter, which essentially freezes the value of the property even if the reference value is changed (this can still be changed by directly calling the instance property)

## View Controllers
- open the storyboard to get a visual representation of your app
- view controller scene navigator on left shows what's going on similar to photoshop layers
- every screen is managed by a View Controller, which is a subclass of UIViewController
- use the Assistant Editor to connect view controller to code
- ctrl + drag elements from storyboard to interface file to insert a property
- viewDidLoad is for things that we want to actually load before they come on screen
- IBOutlet for labels
- your starting storyboard must be set to *Initial* view controller in the attribute inspector
- remember to assign view controllers to the class that should describe what's in the view controller

### Segues
- example
  - take a button on a view controller and set its action (control & drag) onto another view controller
  - set to Present Modally
  - Present is different from Show (navigation controls)

### Passing between view controllers
- ClassOfTheNextViewController \*nameOfTheNextVC = segue.destinationViewController...

lookup lifecycle methods

## Navigation Controllers
- puts a nav bar at the top of the screen

## Table Views
- very versatile class
- forms tables to hold content of any type
- each row is a cell, which holds content
- table views aren't used for columns
- Table View Controller creates a view with a full screen table view
- rows are grouped in sections
- for each new Table View, a Prototype Cell is added which sets the defaults for all cells of that type
- multiple Prototype Cells can be created
- if the number of sections or rows change in your table view, call `[tableView reloadData];`
  - probably in viewWillAppear
- when populating a table view with data from a property of its own class, be sure to add the protocols for `<UITableViewDelegate, UITableViewDataSource>` and set the delegates for both to `self`
- When using a xib or other view to customize the appearance of a table view cell, make sure the prototype cell's style is set to `Custom`

## Auto Layout
- CSS for apps
- Creates universal rules for layout that apply across view sizes
- Uses Anchors to describe layout parameters and constraints
- to be enabled, constraint must be set to .active = YES;
- x axis constraints:  width, left, right
- y axis constraints: height, top, bottom
- only two constraints per axis are ever needed
- center x/y takes the place of left/right or top/bottom
  - center x + width, center y + height

## Constraints in Interface Builder
- control + drag items in the storyboard to other objects or the view itself to open constraint options
- If things seem super broken, check outlets on your storyboards and make sure there aren't any erroneous ones

## Data Stores & Singleton
- a data store is a standard class that is used to hold data
  - usually has just one property, such as an NSMutableArray
  - can hold class methods
- a singleton is a single instance of a data store (or some other class) that is globally available
- class method within the data store that returns an instance of the data store
- EX:
```objc
+ (DataStore *)sharedDataStore {
  static DataStore \*theInstance = nil;

  static dispatch_once_t onceToken; // Checks is this has been run once and marks it as run, doesn't allow to run again once run
  dispatch_once(&onceToken, ^{
    theInstance = [[DataStore alloc] init];
  });
}
```
  - data store should have an init method
  - properties in the data store must have alloc init called for them
- static variables are the same across all instances regardless of what affects each one

## Core Data
- separate files type called Data Model
- persistence that operates at the model level
- saves model classes
- to use:
  - Add Entity (name it in the singular)
  - Attributes are the same as properties
  - Set type
  - After types are modeled, Editor > Create `NSManagedData Subclass` and import this file as needed
  - Make sure the option to "Use scalar properties for primitive data types" is checked !EXCEPT! when using dates
  - The CoreDataProperties files will be deleted and recreated anytime attributes are altered within the Data Model
  - The created header and implementation and header files are able to be altered, and should hold any relevant methods
- `ManagedObjectContext` is the file that knows about the data in the data model
  - a public property you need to create in your header file(s)
- use an `NSFetchRequest fetchRequestWithEntityName:@""` to get an array of objects that specifies a given set of criteria
- to add an object, use `NSEntityDescription insertNewObjectForEntityForName`...
- to save new data persistently in dataStore use `[dataStore saveContext]`
- `deleteObject` deletes objects
- if the core database structure changes, a migration is needed- - uses a mapping file

### Relationships
- arrays within core data are created as Relationships
- for each item type that would be in an array, create a new entity and set the relationship destination appropriately
- create a relationship between entity and the entities it owns (e.g., a customer and items)
- entities can have relationships that return to themselves or go both ways (i.e., inverse relationship enabled) (e.g., manager and subordinates, husband and wife)
- relationship type can be To One or To Many (how many copies of the entity will exist?)
- arrangement can be unordered or ordered and results in creation of NSSet / NSOrderedSet, respectively
- to add or remove an item from a data set, use the methods that are created in the class file for the data set

## Delegates & Protocols
- ??

## Blocks
- extends the functionality of a class without altering the code within that class
- syntax is weird
  - http://fuckingblocksyntax.com

## APIs
- allows you to connect to servers and online services
- CRUD: pretty much every website will have ways to Create, Read, Update, and Destroy data
- verbs act on resources
  - GET requests and pulls down data
  - POST appends to a named resources
  - PUT requests to store a webpage
  - DELETE removes a webpage

## Cocoapods
- open source code packages that can be added to any project through the Podfile and pod install command
- if you're implementing a key feature of your app, write your own code or at the very least understand the code you're importing
- non-essential code, understand it at a high level at least

## OAuth
- allows users to login and authorize app permissions via a secure portal
- make the callback URL somewhat complex and unique
  - this is like a deep link, which opens your app from wherever the URL is called
  - set in the project settings > info > URL Types > URL Schemes
- set the scope parameter in your credential call

## Animations
- class method on UIView
  - animateWithDuration:animations
  - code in block is completion and animation is created automatically
  - initial and ending keyframes are respectively the view on
- use `[self.view layoutIfNeeded]` inside the animation block to update layout view immediately
  - this is necessary when animating movement or transformation
- try to keep animations < 1s, unless doing slow animations for effect
- run multiple animations with `[UIView animateKeyFramesWithDuration:delay:options:animations]` or `[UIView animateWithDuration:delay:options:animations]`
- transformations don't affect layout or constraints

## Touches and Gestures
- defined by Apple
- viewable in the object browser
- after gestures are added to views, user interaction must be enabled for the view in the attribute inspector
- to dismiss a keyboard, use `[self.view endEditing:YES];`

## Xibs
- instead of recreating complex views multiple times, we can create a boilerplate view with a xib file and import this as many times as we'd like

## Notification Center
- Allows disparate parts of the application to talk to one another
- App Delegate sends out several default notifications when the app state changes
- Requires listeners to read the notifications somewhere like the ViewController
- Notifications also exist for keyboard events

## Swift
- It's a whole new world, and it's beautiful
- methods are global, and now called functions: `func`
- private functions and properties have to be declared `private`
- override functions must be declared `override`

### Optionals
- adding a question mark to the end of a type declares it as an optional
- an optional type is allowed to be nil
- attempting to cast a value will choose the optional value type by default to protect against the possibility that the passed value may not be able to be cast
- optional values are wrapped in `Optional(_)`
  - to unwrap the value add `!` to the end of the variable's name where it's called
  - a variable/constant can also be force- unwrapped using `.init` on the value type declaration
- accessing anything in an array will not return an optional
- you can use an `if` statement to check if something is `nil` and then use the value if it's not `nil`
```swift
if let valueThatExists = valueThatMayBeNil {
  // use valueThatExists
}
```
- a value can be force- unwrapped by appending `!` to the end of the variable/constant's name
###Object Oriented Swift
- declare classes with `class`
- created as a Cocoa Touch Class
- ASK FOR EXPLANATION::: `private(set) var friendlyDescription: String`
- to override a setter:
```swift
var productionYear: UInt {
  willSet {                         // default method
    (productionYear) to \(newValue) // newValue is automatically provided in setter method
  }

  didSet {                          // default method
    print(The previous value was \(oldValue)) // oldValue is automatically provided in setter method
  }
}
```
- if you provide a way to set a value, you *must* either make it an optional or provide a default value (which is applied by setting some value when the variable is initialized)
  - you can also init all the variables
```swift
init () {
  // this will init all non- optional variables to some values
  // no return
}
```
- to make a computed property, add treat the var as a function with the return set to the value you want the property to hold
```swift
var ageInYears:UInt {
  return 2016 - birthYear
}
```
- convenience initializers must be prefaced with `convenience`
- it's possible to add functions to a class just as you would to any other Swift file
- class functions are prefaced with `class`
- to init a subclass, set the local variables, then init the superclass with `super.init()`
- call `super.init` when you need to init a new class with the properties of the subclass
- set default values for function arguments in the argument declarations

### Blocks in Swift
- pretty similar to functions
- https://fuckingswiftblocksyntax.com
- `.map` is a handy function for running a block on each item in an array

### Internet in Swift
- Alamofire and SwiftyJSON are the way to go

### Custom Delegates
- Useful in instances where some code might want to _ask_ or _inform_ another class
- The *protocol* formalizes these points and the *delegate* is how they are accessed
  - A *protocol* is a group of methods with a given name
  - The name of a method within a protocol should start with the object delegating
- EX:
```objc
@protocol ColorViewControllerDelegate <NSObject>
@required
-(void)colorViewController:(ColorViewController *)colorViewController didSelectColor:(UIColor *) color;
@end
@interface
@property // This is where the delegate property goes
@end
```

### Swift/Objective-C Interoperability
- Swfit can subclass Objective-C, but not vice-versa
- Swift classes must be subclasses of NSObject or declared with @objc
- Anything with optionals in Swift can't be read by and don't show up in Objective-C
  - Use nullable types are a workaround (e.g., use an unwrapped NSNumber? instead of an NSInt?)

### Memory Management
- Ownership has to do with how things are stored in RAM
  - `strong` (default for objects) references hold an object until the object is removed or the property is assigned to something else.
    - `strong` objects that reference one another will never let go and therefore never be released, creating the potential for memory leaks.
    - Notification Center observers may also be indadvertantly kept enabled if a view controller is retained.
    - *Protip:* _If it's a delegate, make it weak._
    - *Protip:* _If "parent" knows about "child" and "child" knows about "parent", make one of the relationships weak._
    - All local variables, the values of arrays and dictionaries, and references to non-primatives made within a block make strong references by default.
  - `weak` reference's value will revert to `nil` if the object it references (a.k.a. referent) is removed.
  - `assign` (default for primitives) has no strong/weak implications.
  - `copy` makes a strong reference to a copy of the referent. This allows the original referent to be changed without altering the copied value.
    - *Protip:* _If the class of the property has a mutable counterpart (e.g., NSString & NSMutableString), use copy.
  - `unsafe_unretained` acts as `weak`, but does not become `nil` if the referenced object goes away. Just points to deallocated objects (garbage memory). Likely to cause crashes.
  - When there is no strong reference to an object, it will be released from memory.
  - 
- Atomicity has to do with thready saftey, but isn't very relevant anymore.
  - `nonatomic` is generally what you want.
  - `atomic` (default) theoretically makes things safer on older processors.
- Writeability has to do with whether you can write to a property.
  - `readwrite` (default) generates a setter.
  - `readonly` no setter generated for the property.
- Nullability specifies whether it is legal to assign nil to a property.
  - Mostly useful for helping Xcode generate Swfit interfaces for Objective-C code.
  - Only applicable for object types/pointers.
  - `nullable` property comes into Swfit as an optional. (E.g., `String?`)
  - `nonnull` propert comes into Swift as a non-optional. (E.g., `String`)
  - `null_unspecified` (default) property comes into Swift as an implicitly unwrapped optional. (E.g., `String!`)
  - `null_resettable` specifies that the property can be assigned `nil`, and resets the property to some default value. Comes into Swift as an optional.
- Getter and Setter Naming allows you to rename the generated getter and setter methods
  - `getter = <someNewGetter>`
  - `setter = <someNewSetter>`

## CSBS
- The essential CS content which is actually useful in the real world
- Lines of code are not as important as performance, which is measured in Big O notation
  - Big O notation addresses the worst case scenario of an algorithm
  - Determining Big O of an algorithm...
- Arrays vs. Linked Lists
  - Linked Lists are ordered sets that are not in a contiguous portion of memory
  - Each element in a linked list points to the next element, wherever it is
  - Linked lists have better performance for insertions and deletions, but arrays have better performance for access by index
- Sorting Algorithms
  - Many algorithms exist, each with its own pros and cons
  - http://www.sorting-algorithms.com

## Misc.
- It's easier to teach a teacher to code than it is to teach a coder to teach.
- Learn by doing.
- DRY: Don't Repeat Yourself
- SRP: Single Responsibility Principle (everything should do one thing)
- CTRL + Drag a folder or file into terminal to change to that working directory
- To parse the license of some piece of software, use tldrLegal.com
- UIGestureRecognizerState... has several options for sender.state on a gesture's IBAction outlet
- Forward Declarations with `@class` above the `@interface` are used to inform a file that a class exists without importing that class's header
- Freelancing requires incredible work ethic and time management
  - Never accept a freelance job till you've broken it down into the smallest possible parts (with padding)
  - Don't undersell your work or services
  - Always have a contract that fully scopes the app
  - Help the client pare down the idea till its the base feature and you know and agree on exactly what they want
  - Anytime the client asks for server-side functionality, assume the price and time should be five times higher
  - Referrals and Meetups are the best places to get freelance gigs (online freelance marketplaces take too big a cut)
  - Charge more if you have to do design work yourself ($3-4K)
  - Super simple "business card" apps should be ~$4-5K (mass produced apps!)
  - Any good app should take 2-3 months, done right
