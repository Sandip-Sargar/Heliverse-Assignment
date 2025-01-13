// File: backend/app.js

const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');

const app = express();
const PORT = 5000;

// Middleware
app.use(bodyParser.json());

// Database connection
mongoose.connect('mongodb://localhost/hospital-food', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
}).then(() => console.log('Connected to database')).catch((err) => console.error('Database connection error:', err));

// Models
const Patient = mongoose.model('Patient', new mongoose.Schema({
  name: String,
  roomNumber: Number,
  dietChart: {
    morning: String,
    evening: String,
    night: String,
  },
}));

// Routes
app.get('/patients', async (req, res) => {
  const patients = await Patient.find();
  res.json(patients);
});

app.post('/patients', async (req, res) => {
  const patient = new Patient(req.body);
  await patient.save();
  res.status(201).send(patient);
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
