import React, { useState, useEffect } from 'react';
import axios from 'axios';

const RestaurantList = () => {
  const [restaurants, setRestaurants] = useState([]);

  useEffect(() => {
    // Fetch restaurant data from the server
    axios.get('/api/restaurants')
      .then(response => setRestaurants(response.data))
      .catch(error => console.error(error));
  }, []);

  return (
    <div>
      <h2>Restaurants</h2>
      <ul>
        {restaurants.map(restaurant => (
          <li key={restaurant.id}>
            <h3>{restaurant.name}</h3>
            <p>{restaurant.description}</p>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default RestaurantList;
//react
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import RestaurantList from './components/RestaurantList';

const App = () => {
  return (
    <Router>
      <Switch>
        <Route path="/" exact component={RestaurantList} />
        {/* Add more routes for other pages */}
      </Switch>
    </Router>
  );
};

export default App;
//Js
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = process.env.PORT || 5000;

app.use(cors());
app.use(bodyParser.json());

// MongoDB connection
mongoose.connect('mongodb://localhost:27017/foodDelivery', { useNewUrlParser: true, useUnifiedTopology: true });

// Define Restaurant schema and model
const restaurantSchema = new mongoose.Schema({
  name: String,
  description: String,
});

const Restaurant = mongoose.model('Restaurant', restaurantSchema);

// API endpoint for fetching restaurants
app.get('/api/restaurants', async (req, res) => {
  try {
    const restaurants = await Restaurant.find();
    res.json(restaurants);
  } catch (error) {
    res.status(500).json({ error: 'Internal Server Error' });
  }
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
