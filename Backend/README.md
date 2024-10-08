# Back-End

This will go over the creation and alteration of the database along with the rest of the back-end

## Config file

```python
from config import db
```
you need to import the database so that you have something to interact with.

```python
app = Flask(__name__)
CORS(app)
```
This section of code initializes an instance of flask and assigns it to the variable of "app".

The second line (CORS) allows for the transfer of data from the database to the frontend.

```python
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///mydatabase.db"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False
```
configure the SQLite database, relative to the app instance folder.

Flask-SQLAlchemy will track modifications of objects and emit signals, Set it to false when you do not want this to happen.

```python
db = SQLAlchemy(app)
```

This sets up the flask application to work with SQLAlchemy, allowing you to interact with your database using python.

## Models file

```python
class Contact(db.Model):
```

Inheriting from the db.Model instance, allows for proper interaction with the database using "db." commands.

```python
id = db.Column(db.Integer, primary_key = True)
first_name = db.Column(db.String(50), unique = False, nullable = False)
```
this is a declatation of one of the columns that will be in the database table.

```python
def to_json(self):
```
this function is made to put all of the rows of the database into json format so that it can be printed out.

```python
return{
    "id": self.id,
    "firstName": self.first_name,
    }
```
The inside of the function just again, writes it as a json file using self so that it can be accessed across multiple functions.

## Main File
The main file is where the program accounts for all of the different buttons being pressed, which will then lead to different routes.

All of the different functions will have a return of jsonify, to let the user know that the action has been completed

### Listing contacts route

```python
contacts = Contact.query.all()
json_contacts = list(map(lambda x: x.to_json(), contacts))
```
This first line takes the database model/class "Contact" and selects all of the information that is stored within at the given time.

The next line displays it all in a json file so that it is able to be read. It iterates through every line and puts it to a json file.

### Creating the contact

```python
first_name = request.json.get("firstName")
```
using this line for all of the variables that will be included in the database. It receives the inputs from the text boxes and saves them as the variable listed.

```python
if not first_name or not last_name or not email:
return jsonify({"Error: Invalid Input"}), 400
```

This if statement checks the input boxes, and it throws an error if any one of them are not filled out properly. If not it will state "Invald Input"

```python
new_contact = Contact(first_name = first_name, last_name = last_name, email = email)
  try:
    db.session.add(new_contact)
    db.session.commit()
  except Exception as e:
    return jsonify({"mesasge": str(e)}), 400
```
These are the lines that acually commit the new information to the databse. By creating a new variable, it runs all of the new information through the "Contact" class.

Then after this, it will try to add the contact and commit it to the database. The catch is there for if the contact already exists.

### Updating contact

The next function deals with the updating of the contacts.

```python
contact = Contact.query.get(user_id)

  if not contact:
    return jsonify({"message": "Contact Not Found"}), 404
```

To select a contact, they would need to receive the user id. That is what the first line is doing. After that, if there is no contact with the information being looked for, then the "Contact not found" message will pop up

```python
data = request.json
contact.first_name = data.get("firstName", contact.first_name)
```

The data in this context is the data that is being added to the contact being updated

The contact is the information that is already pre-existing.

This "get" line assigns to the specific contact, either the new first name that is being altered or, if empty, the information that was already in the database.

This way, you do not have to refill out the information again even if it remains the same.

```python
db.session.commit()
```

Once again, this line commits the new information to the database

### Deleting a contact

The next function deals with the deleting of the contacts.

It starts the exact same as the last function, where the selection of the contact needs to take place via the "user_id"

```python
db.session.delete(contact)
db.session.commit()
```

All that remains would be to delete the contact, and making sure that the database is made aware

### Main Method

```python
if __name__ == "__main__":
  with app.app_context():
    db.create_all()

  app.run(debug=True)
```

All that is left is to call the creation of the database in the main mehtod!!