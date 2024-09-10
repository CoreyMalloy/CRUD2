# Front-End

This will go over the creation of the front end and all of the things that went in to making it happen

## App File

```JavaScript
const [contacts, setContacts] = useState([]);
const [isModalOpen, setIsModalOpen] = useState(false)
const [currentContact, setCurrentContact] = useState({})
```

These are all of the useStates that will be used throughout this file. They are different variables that will help in navigating the front-end

useStates differ from variables in the sense that they re-render the view(screen) as opposed to variables that just get stored as bits

```JavaScript
useEffect(() => {
    fetchContacts()
  }, []);
```

This useEffect function is used to run as soon as the 'APP.jsx' file starts running. This way we have the contacts without having to do anything to go and get them outselves

```JavaScript
const fetchContacts = async () => {
    const response = await fetch("http://127.0.0.1:5000/contacts")
    const data = await response.json()
    setContacts(data.contacts)
    console.log(data.contacts)
  }
```

This arrow function is whats being called in the 'useEffect' function. This function makes a call to the URL listed above, which is the storage space for our contacts, and saves them as a json file in the variable data

```JavaScript
const closeModal = () => {
    setIsModalOpen(false)
    setCurrentContact({})
  }
```

The next arrow function is used to define the process on which the update modal is closed. So once this modal is closed, the boolean variable turns to false


```JavaScript
const openCreateModal = () => {
    if (!isModalOpen) setIsModalOpen(true)
  }
```

The same thing happens with when it is opened, the variable is set to true.

```JavaScript
<ContactList contacts={contacts} updateContact={openEditModal} updateCallback={onUpdate}/>
      <button onClick={openCreateModal}>Create New Contact</button>
      { isModalOpen && <div className="modal">
        <div className="modal-content">
          <span className="close" onClick={closeModal}>&times;</span>
          <ContactForm existingContact={currentContact} updateCallback={onUpdate}/>
        </div>
      </div>
      }
```

This is the html that is responsible for the functionality of this process

The button, once pressed, opens up the modal available for adding a new contact.

The next few lines defines what exactly is on the modal once it is open, so there will be a button to close the modal that runs through the arrow function "closeModal" and also the rest of the contact form in order to write the information

## Contact Form

The contact form is another file, this will list out the style of what the form will look like. So what exactly is retrieving our information

```JavaScript
const updating = Object.entries(existingContact).length !== 0
```

After creating the useStates for this file (first and last name and email) our goal is to set the standards for updating as opposed to creating a new contact. So if the length of the contact is not already 0, then it is updating.

```JavaScript
const data = {
      firstName,
      lastName,
      email
    }
```

This section that is assigned as 'data' is used to create the structure of the contact.

```JavaScript
const url = "http://127.0.0.1:5000/" + (updating ? `update_contact/${existingContact.id}` : "create_contact")
```
To get the url needed to apply the routes in "main.py", there needs to be this type of statement. This is basically an if statement of sorts that says, the base URL will be the first section in quotes, but after that, if it is updating then the url will end in "update_contact/${existingContact.id}", and if not, it will bring you to the new contact page

```JavaScript
const options = {
      method: updating ? "PATCH" : "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(data)
    }
```

This fuction sets all of the  necessary information that the fetch request might need. It will determine if it is updating or not and also let the fetch request know what type of file is being returned. the body section will set the type of content in the file to json

```JavaScript
const response = await fetch(url, options)
    if (response.status !== 201 && response.status !== 200) {
      const data = await response.json()
      alert(data.message)
    } else{
      updateCallback()
    }
```

This is the fetch call that gets the url and the options and puts them together. It checks to make sure it does not give back an error message and if it does not then it puts it in the 



## Contact List

```JavaScript
const ContactList = ({contacts, updateContact, updateCallback }) =>
```

This line creates the arrow function of "ContactList" with the parameters of the three inside.

```JavaScript
const onDelete = async (id) => {
    try {
      const options = {
        method: "DELETE"
      }
      const response = await fetch(`http://127.0.0.1:5000/delete_contact/${id}`, options)
      if (response.status === 200) {
        updateCallback()
      } else {
        console.error("Failed to delete")
      }
    } catch (error) {
      alert(error)
    }
  }
```

This arrow function passes through the id that is assigned to each individual contact. It assigns the method of the request as the delete method and then goes to the appropriate endpoint.

It checks to make sure that it does not return an error and then updates the call back. 

```JavaScript
{contacts?.map((contact) => (
          <tr key={contact.id}>
            <td>{contact.firstName}</td>
            <td>{contact.lastName}</td>
            <td>{contact.email}</td>
            <td>
              <button onClick={() => updateContact(contact)}>Update</button>
              <button onClick={() => onDelete(contact.id)}>Delete</button>
            </td>
          </tr>
        ))}
```

This arrow function iterates through the array "contacts" and gives the instructions on how to transform it into the output