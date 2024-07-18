# Use Cases
In the use cases the following styles are in use:
1. User interface Views, Lists,  Pages and Editors are displayed **Like This**
2. User actions are shown ```Like This```
## UC1  - The user views an ingredient's details
1. The user launches Nutribase
2. As the default, the **Category List** will open to display the defined categories e.g. *Dairy*,*Fruit*
3. The user selects a category from the **Category List**
4. The **Ingredient List** will open, showing the ingredients from the selected category
5. The user selects an ingredient from the list
6. The **Ingredient Details View**  will open displaying the details of the selected ingredient.
   - By default the calorie unit will be *kCal per 100g* but for some ingredients other units will be more appropriate. For example, *kCal per item* for a cookie.
   - The choice of unit will be fixed for a given ingredient
## UC2 - The user views a meal's details
1. The user launches Nutribase
2. From the navigation menu, the user selects ```Meals```
3. The **Meal List** will be displayed showing the available meals
4. From the meal list, the user selects a meal
5. The **Meal Details** view will open showing:
   - The name of the meal
   - The ingredients that comprise the meal along with each ingredient's calorie contribution
## UC3 - The user adds an ingredient to a meal
### Scenario 1 - Adding an ingredient from the ingredient details view
**Need to make it clear which meal the ingredient is being added to**
1. With the **Ingredient Details** view open the user clicks ```Add```
2. The **Ingredient Editor** will open, showing the selected ingredient. The user will enter the desired amount for the ingredient.
   - The units will be predetermined by the ingredient's entry in the database and will be displayed in the editor 
3. The user clicks ```Add``` to confirm the amount or ```Cancel``` to discard it
4. The user is returned to the **Ingredient Details View**
### Scenario 2 - Adding an ingredient from the meal details view
1. With the **Meal Details View** open the user clicks ```Add```
2. The *Ingredient Picker* will open to show the available list of ingredients
3. The user selects an ingredient and clicks ```Next```
4. The *Ingredient Editor* will open, showing the selected ingredient. The user will enter the desired amount for the ingredient
   - The units will be predetermined by the ingredient's entry in the database and will be displayed in the editor 
5. The user clicks ```Add``` to confirm the amount or ```Cancel``` to discard it
6. The user is returned to the **Meal Details View**
 
## UC - The user modifies a meal ingredient
**What does the user to to get here and what do they do once they are?**
## UC - The user adds a new meal
**What does the user to to get here and what do they do once they are?**
# Technology Considerations
## Data Storage
### Room
Room is part of Android's Jetpack architecture components and provides an abstraction layer over SQLite, making database operations more robust, easier, and efficient. You can either:
* Populate the database at runtime
* Deploy your app with a pre-built database
#### Database Initialisation
##### Building the database at runtime
* Using data from github
* How will we manage database updates? When possible, pull the data files from github adding any new entries
