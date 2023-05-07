# Doctor's Appointment Booking Web-App ( MERN Stack )
Initially we will have this setup for the complete Stack.
```bash
your-app-name/
├── client/
├── server/
└── README.md
```

In this directory structure, you have a `client` folder that contains the front-end code for the app, and a `server` folder that contains the back-end code.

### 1. &nbsp;  Writing Client in React
#### A. Creating the main Client folder.
To create the `client` folder the web app with React and Bootstrap, you can follow these steps:

1. Open a terminal or command prompt in your project's root directory.

2. Run the following command to create a new React app:
    ```bash
    npx create-react-app client
    ```
   This will create a new React app in a folder named `client`.

3. Change your current working directory to the `client` folder:
    ```bash
    cd client
    ```
4. Install Bootstrap as a dependency using the following commands:
    ```bash
    npm install bootstrap
    ```
   This will download and install Bootstrap in the `node_modules` folder of your `client` app.
5. Open the `src/index.js` file in your text editor and add the following line to import the Bootstrap CSS:
    ```js
    import 'bootstrap/dist/css/bootstrap.min.css';
    ```
6. You can then start the development server for your React app using the following commands:
    ```bash
    npm start
    ```

   This will start the development server at `http://localhost:3000` by default.

That's it! You should now have a `client` folder in your MERN stack web app with React and Bootstrap. You can add your own React components, styles, and functionality to this folder to build your web app.

At this point, your directory structure will look similar to this :
```bash
your-app-name/
├── client/
│   ├── node_modules/
│   ├── public/
│   │   ├── index.html
│   │   └── favicon.ico
│   ├── src/
│   │   ├── index.js
│   │   ├── App.js
│   ├── package.json
│   └── package-lock.json
├── server/
└── README.md
```
_*This is excluding some default files that may have been created by the command, for showing the React Demo Starting Page._

#### B. Create Custom react components at the below paths with the codes that follow : 
1. **components/Navbar.js** - `./client/src/components/Navbar.js`
    
    In this file, you would typically define the navigation bar for your web app. Here's an example of what the file could look like:
    ```javascript
    import React from 'react';
    import { Link } from 'react-router-dom';

    const Navbar = () => {
        return (
            <nav className="navbar navbar-expand-lg navbar-light bg-light">
            <Link className="navbar-brand" to="/">Doctor Appointment Booking</Link>
            <button className="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span className="navbar-toggler-icon"></span>
            </button>
            <div className="collapse navbar-collapse" id="navbarNav">
                <ul className="navbar-nav">
                <li className="nav-item">
                    <Link className="nav-link" to="/">Home</Link>
                </li>
                <li className="nav-item">
                    <Link className="nav-link" to="/appointments">Appointments</Link>
                </li>
                <li className="nav-item">
                    <Link className="nav-link" to="/add-appointment">Add Appointment</Link>
                </li>
                </ul>
            </div>
            </nav>
        );
    };

    export default Navbar;
    ```
    In this example, we're using Bootstrap's navbar component to create a responsive navigation bar. The Link component from react-router-dom is used to define the links for each navigation item.

    The navigation items have been customized for a doctor's appointment booking web app, with links to the home page, a list of appointments, and a form to add a new appointment.

    You can customize this navbar further to fit the design and functionality of your web app, and add more navigation items as needed.
2. **components/AppointmentForm.js** - `./client/src/components/AppointmentForm.js`

    In this file, we would typically define a React component that displays a form to add a new appointment to the backend API. Here's an example of what the file could look like:

    ```javascript
    import React, { useState } from 'react';
    import axios from 'axios';

    const AppointmentForm = () => {
        const [formData, setFormData] = useState({
            patientName: '',
            date: '',
            time: ''
        });

        const handleChange = e => {
            setFormData({
            ...formData,
            [e.target.name]: e.target.value
            });
        };

        const handleSubmit = e => {
            e.preventDefault();
            axios.post('/api/appointments', formData)
            .then(res => {
                console.log(res.data);
                setFormData({
                patientName: '',
                date: '',
                time: ''
                });
            })
            .catch(err => {
                console.log(err);
            });
        };

        return (
            <div>
            <h2>Add Appointment</h2>
            <form onSubmit={handleSubmit}>
                <div className="form-group">
                <label htmlFor="patientName">Patient Name</label>
                <input type="text" className="form-control" id="patientName" name="patientName" value={formData.patientName} onChange={handleChange} />
                </div>
                <div className="form-group">
                <label htmlFor="date">Date</label>
                <input type="date" className="form-control" id="date" name="date" value={formData.date} onChange={handleChange} />
                </div>
                <div className="form-group">
                <label htmlFor="time">Time</label>
                <input type="time" className="form-control" id="time" name="time" value={formData.time} onChange={handleChange} />
                </div>
                <button type="submit" className="btn btn-primary">Submit</button>
            </form>
            </div>
        );
    };

    export default AppointmentForm;
    ```
    In this example, we're using React hooks to define a state variable formData that will hold the values of the form fields for adding a new appointment. We're also using the useState hook to update the state whenever a form field changes.

    We're then rendering a form with three input fields for the patient name, date, and time of the appointment. When the form is submitted, we're making an HTTP POST request to the /api/appointments endpoint with the form data using axios. On successful submission, we're resetting the form fields to their initial values.

    You can customize this AppointmentForm component to fit the design and functionality of your web app, and add more features and functionality as needed.

3. **components/AppointmentList.js** - `./client/src/components/AppointmentList.js`

    In this file, we would typically define a React component that displays a list of appointments retrieved from the backend API. Here's an example of what the file could look like:
    ```javascript
    import React, { useState, useEffect } from 'react';
    import axios from 'axios';

    const AppointmentList = () => {
        const [appointments, setAppointments] = useState([]);

        useEffect(() => {
            axios.get('/api/appointments')
            .then(res => {
                setAppointments(res.data);
            })
            .catch(err => {
                console.log(err);
            });
        }, []);

        return (
            <div>
            <h2>Appointments</h2>
            <table className="table">
                <thead>
                <tr>
                    <th scope="col">Patient Name</th>
                    <th scope="col">Date</th>
                    <th scope="col">Time</th>
                    <th scope="col">Actions</th>
                </tr>
                </thead>
                <tbody>
                {appointments.map(appointment => (
                    <tr key={appointment._id}>
                    <td>{appointment.patientName}</td>
                    <td>{appointment.date}</td>
                    <td>{appointment.time}</td>
                    <td>
                        <button className="btn btn-secondary">Edit</button>
                        <button className="btn btn-danger ml-2">Delete</button>
                    </td>
                    </tr>
                ))}
                </tbody>
            </table>
            </div>
        );
    };

    export default AppointmentList;
    ```
    In this example, we're using React hooks to define a state variable appointments that will hold the list of appointments retrieved from the backend API using axios. We're also using the useEffect hook to make an HTTP GET request to the /api/appointments endpoint when the component mounts, and update the appointments state with the response data.

    We're then rendering a table that displays the list of appointments in a tabular format, along with actions to edit or delete each appointment.

    You can customize this AppointmentList component to fit the design and functionality of your web app, and add more features and functionality as needed.

4. **components/AppointmentItem.js** - `./client/src/components/AppointmentItem.js`

    In this file, you would typically define a React component that displays a single appointment item retrieved from the backend API. Here's an example of what the file could look like:
    ```javascript
    import React from 'react';

    const AppointmentItem = ({ appointment }) => {
        return (
            <div className="card my-3">
            <div className="card-body">
                <h5 className="card-title">{appointment.patientName}</h5>
                <p className="card-text">Date: {appointment.date}</p>
                <p className="card-text">Time: {appointment.time}</p>
                <button className="btn btn-danger">Delete</button>
            </div>
            </div>
        );
    };

    export default AppointmentItem;
    ```
    In this example, we're defining a functional component AppointmentItem that takes in an appointment object as a prop. We're then rendering a card with the patient name, appointment date, and time, along with a delete button.

    You can customize this AppointmentItem component to fit the design and functionality of your web app, and add more features and functionality as needed.
5. **components/EditAppointmentForm.js** - `./client/src/components/EditAppointmentForm.js`

    In this file, you would typically define a React component that displays a form to edit an existing appointment retrieved from the backend API. Here's an example of what the file could look like:
    ```javascript
    import React, { useState, useEffect } from 'react';
    import axios from 'axios';

    const EditAppointmentForm = ({ appointment, onUpdate }) => {
        const [formData, setFormData] = useState({
            patientName: appointment.patientName,
            date: appointment.date,
            time: appointment.time
        });

        useEffect(() => {
            setFormData({
            patientName: appointment.patientName,
            date: appointment.date,
            time: appointment.time
            });
        }, [appointment]);

        const handleChange = e => {
            setFormData({
            ...formData,
            [e.target.name]: e.target.value
            });
        };

        const handleSubmit = e => {
            e.preventDefault();
            axios.put(`/api/appointments/${appointment._id}`, formData)
            .then(res => {
                console.log(res.data);
                onUpdate(res.data);
            })
            .catch(err => {
                console.log(err);
            });
        };

        return (
            <div>
            <h2>Edit Appointment</h2>
            <form onSubmit={handleSubmit}>
                <div className="form-group">
                <label htmlFor="patientName">Patient Name</label>
                <input type="text" className="form-control" id="patientName" name="patientName" value={formData.patientName} onChange={handleChange} />
                </div>
                <div className="form-group">
                <label htmlFor="date">Date</label>
                <input type="date" className="form-control" id="date" name="date" value={formData.date} onChange={handleChange} />
                </div>
                <div className="form-group">
                <label htmlFor="time">Time</label>
                <input type="time" className="form-control" id="time" name="time" value={formData.time} onChange={handleChange} />
                </div>
                <button type="submit" className="btn btn-primary">Update</button>
            </form>
            </div>
        );
    };

    export default EditAppointmentForm;
    ```
    In this example, we're defining a functional component EditAppointmentForm that takes in an appointment object and an onUpdate function as props. We're using React hooks to define a state variable formData that will hold the values of the form fields for editing an existing appointment.

    We're then rendering a form with three input fields for the patient name, date, and time of the appointment. When the form is submitted, we're making an HTTP PUT request to the /api/appointments/:id endpoint with the form data using axios, where :id is the ID of the appointment being edited. On successful update, we're calling the onUpdate function passed as a prop with the updated appointment data.

    You can customize this EditAppointmentForm component to fit the design and functionality of your web app, and add more features and functionality as needed.

6.  **utils/api** - `./client/src/utils/api.js`
    
    In this file, you would typically define a set of functions that interact with the backend API to perform CRUD (Create, Read, Update, Delete) operations on the appointments resource. Here's an example of what the file could look like:
    ```javascript
    import axios from 'axios';

    const BASE_URL = '/api/appointments';

    export const getAppointments = () => {
        return axios.get(BASE_URL).then(res => res.data);
    };

    export const addAppointment = appointment => {
        return axios.post(BASE_URL, appointment).then(res => res.data);
    };

    export const updateAppointment = (id, updatedAppointment) => {
        return axios.put(`${BASE_URL}/${id}`, updatedAppointment).then(res => res.data);
    };

    export const deleteAppointment = id => {
        return axios.delete(`${BASE_URL}/${id}`).then(res => res.data);
    };
    ```
    In this example, we're defining four functions:
    - `getAppointments`: sends an HTTP GET request to the `/api/appointments` endpoint to retrieve a list of all appointments.
    - `addAppointment`: sends an HTTP POST request to the `/api/appointments` endpoint to create a new appointment with the data passed in as an argument.
    - `updateAppointment`: sends an HTTP PUT request to the `/api/appointments/:id` endpoint to update the appointment with the given ID with the updated data passed in as an argument.
    - `deleteAppointment`: sends an HTTP DELETE request to the `/api/appointments/:id` endpoint to delete the appointment with the given ID.
    
    Each function returns a promise that resolves to the data returned by the backend API. By defining these functions in a separate file, we can easily import and use them in our 
    React components to interact with the backend API and perform CRUD operations on the appointments resource.

    Note that in this example, we're using the `axios` library to make HTTP requests to the backend API, and we're using a constant `BASE_URL` to define the base URL for the API. You can customize these functions to fit the specific requirements of your web app and backend API.

7.  **routes/appointment** - `./client/src/routes/appointment.js`

    In this file, you would typically define the routes for the appointments-related pages in your web app. Here's an example of what the file could look like:
    ```javascript
    import React from 'react';
    import { Route, Switch } from 'react-router-dom';
    import AppointmentList from '../components/AppointmentList';
    import AppointmentForm from '../components/AppointmentForm';
    import EditAppointmentForm from '../components/EditAppointmentForm';

    const AppointmentRoutes = () => {
        return (
            <Switch>
            <Route exact path="/appointments" component={AppointmentList} />
            <Route exact path="/appointments/new" component={AppointmentForm} />
            <Route exact path="/appointments/:id/edit" component={EditAppointmentForm} />
            </Switch>
        );
    };

    export default AppointmentRoutes;

    In this example, we're defining three routes:
    /appointments: renders the AppointmentList component, which displays a list of all appointments.
    /appointments/new: renders the AppointmentForm component, which displays a form to create a new appointment.
    /appointments/:id/edit: renders the EditAppointmentForm component, which displays a form to edit an existing appointment with the given ID.
    Note that we're using the Route and Switch components from the react-router-dom library to define the routes, and we're importing the AppointmentList, AppointmentForm, and EditAppointmentForm components that we defined earlier. By defining the routes in a separate file, we can easily import and use them in our main App.js component to define the overall structure of our web app.

    ```
8.  **contexts/AppointmentContext** - `./client/src/contexts/AppointmentContext.js`

    In this file, we define the context for managing the state of appointments in our web app. Here's an example of what the file could look like:
    ```javascript
    import React, { createContext, useState, useEffect } from 'react';
    import { getAppointments, addAppointment, updateAppointment, deleteAppointment } from '../utils/api';

    export const AppointmentContext = createContext();

    const AppointmentContextProvider = (props) => {
        const [appointments, setAppointments] = useState([]);

        useEffect(() => {
            getAppointments()
            .then(appointments => setAppointments(appointments))
            .catch(error => console.error(`Error fetching appointments: ${error}`));
        }, []);

        const handleAddAppointment = (appointment) => {
            addAppointment(appointment)
            .then(newAppointment => setAppointments([...appointments, newAppointment]))
            .catch(error => console.error(`Error adding appointment: ${error}`));
        };

        const handleUpdateAppointment = (id, updatedAppointment) => {
            updateAppointment(id, updatedAppointment)
            .then(updatedAppointment => {
                const updatedAppointments = appointments.map(appointment => {
                if (appointment.id === id) {
                    return updatedAppointment;
                } else {
                    return appointment;
                }
                });
                setAppointments(updatedAppointments);
            })
            .catch(error => console.error(`Error updating appointment: ${error}`));
        };

        const handleDeleteAppointment = (id) => {
            deleteAppointment(id)
            .then(() => {
                const updatedAppointments = appointments.filter(appointment => appointment.id !== id);
                setAppointments(updatedAppointments);
            })
            .catch(error => console.error(`Error deleting appointment: ${error}`));
        };

        const appointmentContextData = {
            appointments,
            handleAddAppointment,
            handleUpdateAppointment,
            handleDeleteAppointment
        };

        return (
            <AppointmentContext.Provider value={appointmentContextData}>
               {props.children}
            </AppointmentContext.Provider>
        );
    };

    export default AppointmentContextProvider;
    ```
    In this file, we:
    - Import the createContext, useState, useEffect, and API functions from ../utils/api.
    - Define a context using the createContext function.
    - Define a component called AppointmentContextProvider that manages the state of appointments using the useState hook and the API functions we imported.
    - Use the useEffect hook to fetch appointments from the API when the component mounts.
    - Define functions to handle adding, updating, and deleting appointments using the API functions we imported.
    - Create an object with the state and functions we want to make available in our app through the context.
    - Return the AppointmentContext.Provider component with the context object as its value, and wrap the props.children with it.

9.  **reducers/AppointmentReducer** - `./client/src/reducers/AppointmentReducer.js`

    In the client/src/reducers/AppointmentReducer.js file, we define the reducer function for managing the state of appointments in our web app. Here's an example of what the file could look like:
    ```javascript
    export const appointmentReducer = (state, action) => {
        switch (action.type) {
            case 'SET_APPOINTMENTS':
                return action.payload;
            case 'ADD_APPOINTMENT':
                return [...state, action.payload];
            case 'UPDATE_APPOINTMENT':
                return state.map(appointment => {
                    if (appointment.id === action.payload.id) {
                        return action.payload;
                    } else {
                        return appointment;
                    }
                });
            case 'DELETE_APPOINTMENT':
                return state.filter(appointment => appointment.id !== action.payload);
            default:
                return state;
        }
    };
    ```
    In this file, we define a reducer function that takes a state and an action as arguments, and returns a new state based on the action type.

    In the switch statement, we:

    - Handle the SET_APPOINTMENTS action by returning the action payload (an array of appointments).
    - Handle the ADD_APPOINTMENT action by returning a new state that includes the new appointment (an object).
    - Handle the UPDATE_APPOINTMENT action by returning a new state that replaces the old appointment (an object) with the updated appointment (an object).
    - Handle the DELETE_APPOINTMENT action by returning a new state that filters out the appointment (an object) with the specified ID (a string).

    If the action type does not match any of the cases, we simply return the existing state.
10.  **actions/AppointmentActions** - `./client/src/actions/AppointmentActions.js`
     
     In the ./client/src/actions/AppointmentActions.js file, there should be action creators for making API requests to the backend and updating the state in the appointment context.
 
     Here's an example of what the file could look like:
     ```javascript
     import axios from 'axios';
 
     // Constants
     export const GET_APPOINTMENTS = 'GET_APPOINTMENTS';
     export const ADD_APPOINTMENT = 'ADD_APPOINTMENT';
     export const DELETE_APPOINTMENT = 'DELETE_APPOINTMENT';
     export const UPDATE_APPOINTMENT = 'UPDATE_APPOINTMENT';
 
     // Action creators
     export const getAppointments = () => async dispatch => {
         try {
             const res = await axios.get('/api/appointments');
             dispatch({ type: GET_APPOINTMENTS, payload: res.data });
         } catch (err) {
             console.error(err);
         }
     };
 
     export const addAppointment = appointment => async dispatch => {
         try {
             const res = await axios.post('/api/appointments', appointment);
             dispatch({ type: ADD_APPOINTMENT, payload: res.data });
         } catch (err) {
             console.error(err);
         }
     };
 
     export const deleteAppointment = id => async dispatch => {
         try {
             await axios.delete(`/api/appointments/${id}`);
             dispatch({ type: DELETE_APPOINTMENT, payload: id });
         } catch (err) {
             console.error(err);
         }
     };
 
     export const updateAppointment = appointment => async dispatch => {
         try {
             const res = await axios.put(`/api/appointments/${appointment._id}`, appointment);
             dispatch({ type: UPDATE_APPOINTMENT, payload: res.data });
         } catch (err) {
             console.error(err);
         }
     };
     ```
     In this example, there are four action creators defined:
 
     - getAppointments: retrieves all the appointments from the backend and dispatches a GET_APPOINTMENTS action with the payload set to the retrieved data.
     - addAppointment: sends a POST request to the backend to add a new appointment and dispatches an ADD_APPOINTMENT action with the payload set to the newly created appointment.
     - deleteAppointment: sends a DELETE request to the backend to delete an appointment with the given ID and dispatches a DELETE_APPOINTMENT action with the payload set to the ID of the deleted appointment.
     - updateAppointment: sends a PUT request to the backend to update an appointment with the given data and dispatches an UPDATE_APPOINTMENT action with the payload set to the updated appointment data.
 
11.  **types/AppointmentTypes** - `./client/src/types/AppointmentTypes.js`
 
     In the ./client/src/types/AppointmentTypes.js file, there should be constants that define the types of actions that can be dispatched to the appointment reducer.
 
     Here's an example of what the file could look like:
     ```javascript
     export const GET_APPOINTMENTS = 'GET_APPOINTMENTS';
     export const ADD_APPOINTMENT = 'ADD_APPOINTMENT';
     export const DELETE_APPOINTMENT = 'DELETE_APPOINTMENT';
     export const UPDATE_APPOINTMENT = 'UPDATE_APPOINTMENT';
     ```
     In this example, there are four constants defined:
 
     - GET_APPOINTMENTS: used to retrieve all the appointments from the backend and update the state in the appointment context.
     - ADD_APPOINTMENT: used to add a new appointment to the state in the appointment context.
     - DELETE_APPOINTMENT: used to delete an appointment from the state in the appointment context.
     - UPDATE_APPOINTMENT: used to update an appointment in the state in the appointment context.

At this point, your directory structure will look similar to this :
```bash
your-app-name/
├── client/
│   ├── node_modules/
│   ├── public/
│   │   ├── index.html
│   │   └── favicon.ico
│   ├── src/
│   │   ├── index.js
│   │   ├── App.js
│   │   ├── components/
│   │   │   ├── Navbar.js
│   │   │   ├── AppointmentForm.js
│   │   │   ├── AppointmentList.js
│   │   │   ├── AppointmentItem.js
│   │   │   └── EditAppointmentForm.js
│   │   ├── utils/
│   │   │   └── api.js
│   │   ├── routes/
│   │   │   └── appointment.js
│   │   ├── contexts/
│   │   │   └── AppointmentContext.js
│   │   ├── reducers/
│   │   │   └── AppointmentReducer.js
│   │   ├── actions/
│   │   │   └── AppointmentActions.js
│   │   └── types/
│   │       └── AppointmentTypes.js
│   ├── package.json
│   └── package-lock.json
└── README.md
```
_*This is excluding some default files that may have been created by the command, for showing the React Demo Starting Page._

## 2. Writing the Server Code in Express.js in Node.js and MongoDB Code
#### A. To create the `server` folder for your MERN stack web app, you can follow these steps:
1. Open your terminal and navigate to the root directory of your project.

2. Run the following command to create a new folder named `server`:

```
mkdir server
```

3. Move into the `server` folder using the following command:

```
cd server
```

4. Inside the `server` folder, you will need to create several files and folders for your server-side code, including a `package.json` file to manage your server-side dependencies.

5. Run the following command to initialize a new `package.json` file in the `server` folder:

```
npm init -y
```

6. Next, you will need to install the necessary dependencies for your server-side code. You can install the following packages using the `npm install` command:

- `express`: a popular Node.js framework for building web applications
- `mongoose`: a package for working with MongoDB databases
- `cors`: a package for enabling cross-origin resource sharing

Here's an example command to install these packages:

```
npm install express mongoose cors
```

7. After installing these packages, you can create your server-side code inside the `server` folder. For example, you might create a `server.js` file to define your server's routes and endpoints, a `models` folder to define your data models using Mongoose, and a `routes` folder to define your API routes.

8. Once you have created your server-side code, you can start your server using the following command:

```
node server.js
```

### B. Create server parts at the below paths with the codes that follow :

1. **server/index** - `./server/index.js`
    
    Main code for Express Server
    ```javascript
    const express = require('express');
    const cors = require('cors');
    const mongoose = require('mongoose');
    const bodyParser = require('body-parser');
    const Appointment = require('./models/appointment');

    // Create express app
    const app = express();

    // Middlewares
    app.use(cors());
    app.use(bodyParser.json());

    // Connect to MongoDB
    mongoose.connect('mongodb://localhost:27017/appointments', { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log('MongoDB Connected'))
    .catch((err) => console.log(err));

    // Create an appointment
    app.post('/appointments', (req, res) => {
        const appointment = new Appointment({
            name: req.body.name,
            email: req.body.email,
            date: req.body.date,
            time: req.body.time
        });

        appointment.save()
            .then((result) => res.json(result))
            .catch((err) => console.log(err));
    });

    // Get all appointments
    app.get('/appointments', (req, res) => {
        Appointment.find()
            .then((result) => res.json(result))
            .catch((err) => console.log(err));
    });

    // Get a single appointment
    app.get('/appointments/:id', (req, res) => {
        const id = req.params.id;
        Appointment.findById(id)
            .then((result) => res.json(result))
            .catch((err) => console.log(err));
    });

    // Update an appointment
    app.put('/appointments/:id', (req, res) => {
        const id = req.params.id;
        const { name, email, date, time } = req.body;
        Appointment.findByIdAndUpdate(id, { name, email, date, time })
            .then(() => res.json({ message: 'Appointment updated successfully' }))
            .catch((err) => console.log(err));
    });

    // Delete an appointment
    app.delete('/appointments/:id', (req, res) => {
        const id = req.params.id;
        Appointment.findByIdAndDelete(id)
            .then(() => res.json({ message: 'Appointment deleted successfully' }))
            .catch((err) => console.log(err));
    });

    // Start server
    const PORT = process.env.PORT || 5000;
    app.listen(PORT, () => console.log(`Server started on port ${PORT}`));
    ```
    In the above code, we first import and use the necessary libraries like `express`, `cors`, `mongoose` and `body-parser`. We then connect to MongoDB using `mongoose.connect()`.

    We then define the various routes for the CRUD operations on appointments - creating an appointment (`POST /appointments`), getting all appointments (`GET /appointments`), getting a single appointment (`GET /appointments/:id`), updating an appointment (`PUT /appointments/:id`) and deleting an appointment (`DELETE /appointments/:id`).

    Finally, we start the server on the specified port using `app.listen()`. Note that in this example, we are assuming that the MongoDB server is running on `localhost` at the default port `27017`.

2. **models/Appointment** - `./models/Appointment.js`
    Based on the code provided in the server-side code, the Appointment model should be defined in the ./models/Appointment.js file as follows:
    ```javascript
    const mongoose = require('mongoose');

    const AppointmentSchema = new mongoose.Schema({
        patientName: {
            type: String,
            required: true,
        },
        doctorName: {
            type: String,
            required: true,
        },
        date: {
            type: Date,
            required: true,
        },
        time: {
            type: String,
            required: true,
        },
        kind: {
            type: String,
            required: true,
        },
    });

    const Appointment = mongoose.model('Appointment', AppointmentSchema);

    module.exports = Appointment;
    ```
    This model defines the schema for the `Appointment` object, which has properties for `patientName`, `doctorName`, `date`, `time`, and `kind`. The `Appointment` model is created using the `mongoose.model` method and exported for use in other parts of the application.

3. **routes/appointment** - `./routes/appointment.js`
    This file should be defined as follows:
    ```javascript
    const express = require('express');
    const router = express.Router();
    const Appointment = require('../models/Appointment');

    // Get all appointments
    router.get('/', async (req, res) => {
        try {
            const appointments = await Appointment.find();
            res.json(appointments);
        } catch (err) {
            res.status(500).json({ message: err.message });
        }
    });

    // Get a single appointment
    router.get('/:id', getAppointment, (req, res) => {
        res.json(res.appointment);
    });

    // Create an appointment
    router.post('/', async (req, res) => {
        const appointment = new Appointment({
            patientName: req.body.patientName,
            doctorName: req.body.doctorName,
            date: req.body.date,
            time: req.body.time,
            kind: req.body.kind
        });

        try {
            const newAppointment = await appointment.save();
            res.status(201).json(newAppointment);
        } catch (err) {
            res.status(400).json({ message: err.message });
        }
    });

    // Update an appointment
    router.patch('/:id', getAppointment, async (req, res) => {
        if (req.body.patientName != null) {
            res.appointment.patientName = req.body.patientName;
        }
        if (req.body.doctorName != null) {
            res.appointment.doctorName = req.body.doctorName;
        }
        if (req.body.date != null) {
            res.appointment.date = req.body.date;
        }
        if (req.body.time != null) {
            res.appointment.time = req.body.time;
        }
        if (req.body.kind != null) {
            res.appointment.kind = req.body.kind;
        }

        try {
            const updatedAppointment = await res.appointment.save();
            res.json(updatedAppointment);
        } catch (err) {
            res.status(400).json({ message: err.message });
        }
    });

    // Delete an appointment
    router.delete('/:id', getAppointment, async (req, res) => {
        try {
            await res.appointment.remove();
            res.json({ message: 'Deleted Appointment' });
        } catch (err) {
            res.status(500).json({ message: err.message });
        }
    });

    // Middleware function to get an appointment by ID
    async function getAppointment(req, res, next) {
        let appointment;
        try {
            appointment = await Appointment.findById(req.params.id);
            if (appointment == null) {
                return res.status(404).json({ message: 'Cannot find appointment' });
            }
        } catch (err) {
            return res.status(500).json({ message: err.message });
        }

        res.appointment = appointment;
        next();
    }

    module.exports = router;
    ```
    This router defines the endpoints for handling CRUD operations on the `Appointment` objects. It includes middleware to get an appointment by ID and uses the `Appointment` model for database operations.

4. **config/db** - `./config/db.js`
    Based on the code provided earlier, the ./config/db.js file should contain the following code:
    ```javascript
    const mongoose = require('mongoose');

    const connectDB = async () => {
        try {
            await mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost:27017/appointments', {
            useNewUrlParser: true,
            useUnifiedTopology: true,
            useFindAndModify: false,
            useCreateIndex: true
            });
            console.log('MongoDB connected');
        } catch (err) {
            console.error(err.message);
            process.exit(1);
        }
    };

    module.exports = connectDB;
    ```
   This code exports a function named `connectDB` which connects to the MongoDB database using Mongoose. The URI for the database connection is taken from the environment variable `MONGODB_URI` if it exists, otherwise it defaults to `mongodb://localhost:27017/appointments`. The options passed to `mongoose.connect()` ensure that various features such as the new URL parser, unified topology, and index creation are enabled. If the connection is successful, a message is logged to the console. If an error occurs, the error message is logged to the console and the process exits with a non-zero exit code.

# Conclusion
Your directory structure at this point should look something like: 

```bash
your-app-name/
├── client/
│   ├── node_modules/
│   ├── public/
│   │   ├── index.html
│   │   └── favicon.ico
│   ├── src/
│   │   ├── index.js
│   │   ├── App.js
│   │   ├── components/
│   │   │   ├── Navbar.js
│   │   │   ├── AppointmentForm.js
│   │   │   ├── AppointmentList.js
│   │   │   ├── AppointmentItem.js
│   │   │   └── EditAppointmentForm.js
│   │   ├── assets/
│   │   │   ├── css/
│   │   │   │   ├── bootstrap.min.css
│   │   │   │   └── custom.css
│   │   │   └── js/
│   │   │       └── bootstrap.min.js
│   │   ├── utils/
│   │   │   └── api.js
│   │   ├── routes/
│   │   │   └── appointment.js
│   │   ├── contexts/
│   │   │   └── AppointmentContext.js
│   │   ├── reducers/
│   │   │   └── AppointmentReducer.js
│   │   ├── actions/
│   │   │   └── AppointmentActions.js
│   │   └── types/
│   │       └── AppointmentTypes.js
│   ├── package.json
│   └── package-lock.json
├── server/
│   ├── node_modules/
│   ├── index.js
│   ├── models/
│   │   └── Appointment.js
│   ├── routes/
│   │   └── appointment.js
│   ├── config/
│   │   └── db.js
│   ├── package.json
│   └── package-lock.json
└── README.md
```
In this directory structure, you have a `client` folder that contains the front-end code for the app, and a `server` folder that contains the back-end code. The `client` folder has a `src` directory that contains the React components, utility functions, and other related files. The `server` folder has an `index.js` file that sets up the server and connects to the database, and a `routes` directory that contains the API routes for the app.

In the `client/src` directory, you have a `components` directory that contains the React components for the app, an `assets` directory that contains CSS and JavaScript files for Bootstrap, a `utils` directory that contains a file for making API calls to the server, a `routes` directory that contains the API routes for the app, a `contexts` directory that contains a React context for managing appointments, a `reducers` directory that contains the appointment reducer, an `actions` directory that contains the appointment actions, and a `types` directory that contains the appointment action types.

You can customize this directory structure to suit your specific needs and coding style.
