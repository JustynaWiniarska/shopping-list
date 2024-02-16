# Shopping List in Vue3

### A shopping list application that allows a user to search for an item, add items, check them off, and delete them.

<img width="766" alt="Screenshot 2024-01-14 at 11 45 59 PM" src="https://github.com/JustynaWiniarska/shopping-list/assets/22163656/11df20e4-817e-4d5b-a384-ec0e34c83acc">

#### Added items can be checked off, unchecked again, and deleted from the list.
<img width="770" alt="Screenshot 2024-01-14 at 11 48 34 PM" src="https://github.com/JustynaWiniarska/shopping-list/assets/22163656/d8dc490c-6969-40a7-bcec-9e6c91e277f6">

#### Requirements
- Entering more than two characters in the input should show a list of partially matching items (starting with the same characters)
- Clicking an item in the list of partially matching items should add it to the list
- Adding the same item multiple times is allowed
- Pressing the 'X' next to an item should delete it from the list
- Pressing the '✓' next to an item should check it off (i.e. strikethrough text and partially grey out text/buttons)
- Pressing the '✓' next to a checked-off item should uncheck it again

#### API endpoints
- Method: GET
- URL: https://api.frontendeval.com/fake/food/:food
- Example request URL: https://api.frontendeval.com/fake/food/mi
- Example response: ['milk', 'milkshake', 'mint', 'mixed herbs']
- Only accepts items with a minimum of two characters (e.g. 'mi' is fine, 'm' will return an empty array')
- Debounces the input

#### Additional Features
- Adds a way for the user to adjust quantity for each item by clicking '+' and '-' buttons.
- Makes the autocomplete list navigable with a keyboard: pressing up/down on the keyboard changes the highlighted item in the list of partially matching items (e.g. press down = highlight the next item, press up = highlight the previous item). Pressing enter with an item highlighted adds it to the list.