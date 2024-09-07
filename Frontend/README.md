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




## Contact Form

## Contact List