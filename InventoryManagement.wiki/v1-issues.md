## Angular Front-end
1. Equipment Type and Equipment Edit fields are not removed from updated items array if they are edited and set back to empty. In other words, if you were to type a new updated comment in the form and you didn't want to save the update, there will be an empty string for that key in the updated items array so when submit is hit, it will set the comment to an empty string in the database (ie. "").
2. Loans page for a user does not have an option to select a due date.

## API Back-end
1. There is no input validation for any of the controllers. If you were to enter an equipment with the same name, it will add a duplicate.
2. No validation on delete for equipment. If an item is loaned to a user, you can still delete the item. May cause broken links since the user document contains the MongoId of the equipment that is currently loaned out and previous loans.