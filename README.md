# Cat Pic Puzzle - Collection Views

![snapshot](https://s3.amazonaws.com/learn-verified/cat-puzzle-snapshot.png)

## Objectives
1. Build a picture game using a collection view controller
2. Become familiar with data handling, view layout, and subclassing reusable views
3. Determine the game winning sequence to trigger a segue and modally present another view controller

# Part 1

### About Collection Views

A collection view is an organized view with a customizable layout that manages an ordered collection of data items. Unlike a table view which is restricted to displaying only rows, collection views can display rows, columns, and other unique layouts.

There are two key parts to a collection view:
 1. __Data__ (the information displayed in the view)
 2. __Layout__ (how the information is displayed)

This lab will focus on implementing the necessary protocols to satisfy these key parts. Collection views can be difficult to understand at first but Apple's documentation is always a great resource for learning about the different parts of a collection view. Check out these pages for clarification on steps within the lab.

 * [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview)
 * [UICollectionViewController](https://developer.apple.com/reference/uikit/uicollectionviewcontroller)
 * [UICollectionViewDataSource](https://developer.apple.com/reference/uikit/uicollectionviewdatasource)
 * [UICollectionViewDelegate](https://developer.apple.com/reference/uikit/uicollectionviewdelegate)
 * [UICollectionViewDelegateFlowLayout](https://developer.apple.com/reference/uikit/uicollectionviewdelegateflowlayout)

### 1. Set Up The Collection View Controller in Interface Builder

 * Open the project file and navigate to the `Main.storyboard` file. You will notice that it's empty.
 * Go to the Utilities sidebar and search the Object library for a Collection View Controller object.
 * Drag a Collection View Controller object on to the canvas.
 * With the controller selected, go the the Attributes inspector and set the controller as the initial view controller.

With the controller selected in the main storyboard, take a look at the Connections inspector. You will notice that the `dataSource` protocol and the `delegate` protocol are connected as referencing outlets. A collection view is like a table view in that it has protocols for providing data to the view and for listening to changes on the view (e.g., if a user selects a cell, etc.).

These referencing outlets are created automatically when a Collection View Controller object is dragged on to the canvas in interface builder. This means the controller conforms to the protocols and becomes the data source object and the delegate object of the collection view. These connections also carry over when a custom class is connected to the controller in the Identity inspector. That's what you will do next.

### 2. Create a Subclass of UICollectionViewController

 * Create a new swift file called `CollectionViewController` and save it to the project file.
 * Change `import Foundation` to `import UIKit`.
 * Create a class called `CollectionViewController` that inherits from `UICollectionViewController` and leave the body of the class empty for the moment.
 * Go back to `Main.storyboard` and connect the custom class you just created to the Collection View Controller via the Identity inspector.

### 3. Add the Required DataSource Methods

Since the `CollectionViewController` class conforms to the data source protocol via interface builder, a minimum of two methods are required to provide data to the collection view:

 `collectionView(_:numberOfItemsInSection:)`<br>
 `collectionView(_:cellForItemAt:)`

The first data source method, `collectionView(_:numberOfItemsInSection:)` returns an integer with the number of items to display in a section. The second method, `collectionView(_:cellForItemAt:)`, returns a `UICollectionViewCell` with content to display at the specified index path.

 * Add the two required methods to the `CollectionViewController` class.
 * For now, return `12` in `collectionView(_:numberOfItemsInSection:)`
 * Before defining `collectionView(_:cellForItemAt:)`, go to `Main.storyboard`. Select the collection view cell from the document outline and set the Reuse Identifier as "puzzleCell".
 * Back inside `collectionView(_:cellForItemAt:)`, declare a `UICollectionViewCell` using `dequeueReusableCell(withReuseIdentifier:for:)`. The dequeue method is called on the `collectionView` property of the controller class (i.e., `self.collectionView.dequeue...`).
 * Update the background color property of the cell by assigning a color value (e.g., `UIColor.purple`).
 * Return the cell to complete the `collectionView(_:cellForItemAt:)` method.
 * Build and run the application. Your collection view should display 12 purple cells.

### 4. Add a Subclass of UICollectionViewCell

The collection view cell needs to include an image view in order to display the different image slices of the cat picture puzzle. An image view could be added to the cell directly inside of `collectionView(_:cellForItemAt:)` but a better approach is to subclass a `UICollectionViewCell`. This way the subclass can handle all the functionality that only relates to the cell. This approach is more organized and scalable. The good news is the subclass has already been created for you.

 * Go to the `CollectionViewCell.swift` file and review the subclass of `UICollectionViewCell`. Note that there's an `imageView` property and two initializers that call the same `commonInit()` method.
 * Go to `Main.storyboard` and select the collection view cell in the document outline.
 * With the cell selected, add `CollectionViewCell` as the custom class in the Identity inspector.
 * Go back to the `CollectionViewController.swift` file to make some edits to `collectionView(_:cellForItemAt:)`.
 * Replace the statement that assigns the background color with a statement that accesses the `imageView` property of the custom cell class. Assign a value of `UIImage(named: "cats")`. See the hint below to get this part working.
 * Build and run the application. You should see 12 repeating cat pictures.

_**Hint:**_ Even though the custom cell class was connected to the cell in interface builder, `dequeueReusableCell(withReuseIdentifier:for:)` still returns a cell of type `UICollectionViewCell`. Is there something you could add on to the end of this statement to indicate the return should be of the custom type `CollectionViewCell`? Perhaps a force downcast could help?
