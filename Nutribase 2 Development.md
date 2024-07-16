# Use Cases
## UC1  - The user views the defined food categories
1. The user launches Nutribase
2. As the default, the **categories view** will open to display the defined food categories
## UC2  - The user views the foods in a category
1. The user selects a category from the categories view
2. The **foods view** will open, showing the foods from the selected category
## UC3 - The user views the details of a food item
1. The user opens the foods view and selects an item
2. The **food details** view will open, displaying the following details for the selected food:
       * Calories per unit. By default this will be *kCal per 100g* but for some foods other units will be more appropriate. For example, *kCal per item* for a cookie
## UC4 - The user views the available meals
1. The user launches Nutribase
2. From the navigation menu, the user selects *Meals*
3. The **meal list** will be displayed showing the available meals
## UC5 - The user views a meal's details
1. From the meal list, the user selects a meal
2. The **meal details** view will open showing:
   * The name of the meal
   * A list of the foods that comprise the meal
   * Each food will have its calorie contribution displayed
## UC - The user adds an ingredient to a meal - scenario 1
### Add from food details
* From the food details *
## UC - The user adds an ingredient to a meal - scenario 2
### Add from meal details
## UC - The user modifies a meal ingredient
Navigation by one of the 
## The user adds a new meal



# Technology Considerations
## Data Storage
### Room
Room is part of Android's Jetpack architecture components and provides an abstraction layer over SQLite, making database operations more robust, easier, and efficient. You can either:
* Populate the database at runtime
* Deploy your app with a pre-built database
#### Database Initialisation
##### Building the database at runtime
* Using data from github
* How will we manage database updates?
  When possible, pull the data files from github adding any new entries
##### Using a prebuilt database
It's possible to deploy a pre-built Room database with an Android application. This approach can be useful if you want to initialize your database with predefined data. Here’s a step-by-step guide on how to achieve this:

1. First, create the database using *SQLite* tools on your development machine. You can use tools like SQLite Database Browser or Android Studio's SQLite Database Inspector to create and populate the database
2. Place the pre-built database file in the `assets` folder of your Android project. The `assets` folder should be located in the `src/main` directory of your project
3. Modify your Room database class to copy the database from the assets folder to the device's internal storage when the app is first launched
