# Use Cases
## UC1  - The user views a food's details
1. The user launches Nutribase
2. As the default, the **Category List** will open to display the defined food categories
3. The user selects a category from the **Category List**
4. The **Food List** will open, showing the foods from the selected category
5. The user selects a food from the list
6. The **Food Details View**  will open displaying the details of the selected food.
   - By default the calorie unit will be *kCal per 100g* but for some foods other units will be more appropriate. For example, *kCal per item* for a cookie.
   - The choice of unit will be fixed for a given food
## UC4 - The user views the available meals
1. The user launches Nutribase
2. From the navigation menu, the user selects *Meals*
3. The **Meal List** will be displayed showing the available meals
## UC5 - The user views a meal's details
1. From the meal list, the user selects a meal
2. The **Meal Details** view will open showing:
   - The name of the meal
   - The foods that comprise the meal along with each ingredient's calorie contribution
## UC6 - The user adds an ingredient to a meal
### Scenario 1 - Adding an ingredient from the food details view
**Need to make it clear which meal the ingredient is being added to**
1. With the food details view open the user clicks *Add* (or *+*)
2. The **Ingredient Editor** will open, showing the selected ingredient. The user will enter the desired amount for the ingredient.
   - The units will be predetermined by the ingredient's entry in the database and will be displayed in the editor 
3. The user clicks *Add* to confirm the amount or *Cancel* to discard it
4. The user is returned to the **Food Details View**
### Scenario 2 - Adding an ingredient from the meal details view
1. With the **Meal Details View** open the user clicks *Add* (or *+*)
2. The *Ingredient Picker* will open to show the available list of ingredients
3. The user selects an ingredient and clicks *Next*
4. The *Ingredient Editor* will open, showing the selected ingredient. The user will enter the desired amount for the ingredient.
   - The units will be predetermined by the ingredient's entry in the database and will be displayed in the editor 
 
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
