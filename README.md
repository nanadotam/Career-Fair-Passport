# Career-Fair-Passport

Building a web app to act as a passport for students attending events.

### Technical Requirements and Structure

1. **User Authentication and Verification:**
   - **Students:** Need to register with their student email and ID.
   - **Supervisors:** Need an authentication mechanism to scan and stamp passports.

2. **QR Code Generation and Scanning:**
   - **QR Codes:** Each event will generate a unique QR code.
   - **Scanning:** Supervisors will scan the QR code using their devices to stamp the student's passport.

3. **Database:**
   - Store student details, event details, attendance records, and QR codes.

### Tools and Frameworks

#### Frontend:
1. **React.js:** For building a dynamic and responsive user interface.
2. **Redux:** For state management if the app becomes complex.
3. **Axios:** For making HTTP requests to the backend.

#### Backend:
1. **Node.js with Express.js:** For building a RESTful API.
2. **MongoDB:** A NoSQL database for storing user and event data.
3. **Passport.js:** For authentication purposes.
4. **QRCode.js:** For generating QR codes.
5. **jsonwebtoken:** For secure authentication and authorization.

#### Additional Tools:
1. **Firebase Authentication:** Can be an alternative for managing user authentication.
2. **Heroku or Vercel:** For deploying the web application.

### Architecture and Workflow

1. **User Registration and Login:**
   - Students register with their student email and ID.
   - Supervisors have a different login mechanism with additional permissions.

2. **Dashboard:**
   - **Students:** View their events and attendance records.
   - **Supervisors:** View events, scan QR codes, and stamp passports.

3. **Event Management:**
   - Admins create events with unique QR codes.
   - Students register for events.

4. **QR Code Scanning and Stamping:**
   - Supervisors scan the student's QR code at the event.
   - The system verifies the QR code and stamps the student's passport.

### Implementation Steps

1. **Set Up Frontend:**
   - Initialize a React.js project.
   - Create components for registration, login, dashboard, and QR code scanning.
   - Use QRCode.js to generate QR codes.

2. **Set Up Backend:**
   - Initialize a Node.js project with Express.js.
   - Create endpoints for user registration, login, event creation, and QR code verification.
   - Use MongoDB to store user data, event details, and attendance records.

3. **User Authentication:**
   - Implement user authentication using Passport.js or Firebase.
   - Secure endpoints using JWT tokens.

4. **QR Code Functionality:**
   - Generate QR codes for events.
   - Implement QR code scanning functionality on the frontend.
   - Verify QR codes and update attendance records on the backend.

### Example Code Snippets

#### Frontend (React.js)

**Generating QR Code:**
```jsx
import QRCode from 'qrcode.react';

function EventQRCode({ eventId }) {
  const qrCodeValue = `https://yourapp.com/event/${eventId}`;
  
  return (
    <div>
      <QRCode value={qrCodeValue} />
    </div>
  );
}
```

**Scanning QR Code (Using a library like react-qr-reader):**
```jsx
import { useState } from 'react';
import QrReader from 'react-qr-reader';

function QRScanner({ onScan }) {
  const handleScan = data => {
    if (data) {
      onScan(data);
    }
  };

  const handleError = err => {
    console.error(err);
  };

  return (
    <QrReader
      delay={300}
      onError={handleError}
      onScan={handleScan}
    />
  );
}
```

#### Backend (Node.js with Express.js)

**User Registration:**
```javascript
const express = require('express');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const User = require('./models/User');

const router = express.Router();

router.post('/register', async (req, res) => {
  try {
    const hashedPassword = await bcrypt.hash(req.body.password, 10);
    const user = new User({
      email: req.body.email,
      studentId: req.body.studentId,
      password: hashedPassword
    });
    await user.save();
    res.status(201).send('User registered');
  } catch (error) {
    res.status(400).send(error.message);
  }
});

module.exports = router;
```

**Event Creation with QR Code Generation:**
```javascript
const express = require('express');
const QRCode = require('qrcode');
const Event = require('./models/Event');

const router = express.Router();

router.post('/create-event', async (req, res) => {
  try {
    const event = new Event({
      name: req.body.name,
      date: req.body.date
    });
    const savedEvent = await event.save();
    const qrCodeUrl = await QRCode.toDataURL(`https://yourapp.com/event/${savedEvent._id}`);
    savedEvent.qrCode = qrCodeUrl;
    await savedEvent.save();
    res.status(201).send(savedEvent);
  } catch (error) {
    res.status(400).send(error.message);
  }
});

module.exports = router;
```
