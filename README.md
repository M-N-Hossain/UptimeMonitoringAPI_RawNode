
# Uptime Monitoring API

This project implements an API to monitor user-defined checks for API uptime and send mobile notifications via SMS when the status changes (up or down).

## Functionality

Users can register and create API checks.
Checks are validated based on provided properties (protocol, URL, method, timeout seconds, success codes).
The system regularly performs checks against the specified URLs.
If a check's status changes (up to down, or vice versa), the user is notified via SMS.
## Prerequisites

Node.js and npm (or yarn) installed on your development machine.
A mobile phone number capable of receiving SMS messages (for testing notifications).
A Twilio account (optional, but required for sending SMS notifications).

        
    


## How to run
Please follow the below instructions to run this project in your machine:

1. Clone the project

```bash
  git clone https://link-to-project
```

2. Go to the project directory

```bash
  cd my-project
```

3. Install dependencies

```bash
  npm install
```
4. Configuration

For Twilio SMS notifications, go to environments file in the project helper directory and made the changes accordingly:
```

 twilio: {
    fromPhone: setNumber,
    accountSid: your_twilio_account_sid,
    authToken: your_twilio_auth_token ,
  }
```

5. Start the server
```bash
  npm start
```
6. Your app should be available in http://localhost:3000


## API Overview
### User Registration
To register a user, send a POST request to the following endpoint:
```
handler._users.post = (requestProperties, callback) => {
  // Validates and creates a user
};
```
You must provide:

* firstName: A valid string
* lastName: A valid string
* phone: A valid 8-digit phone number
* password: A string
* tosAgreement: Boolean, true if the user agrees to the terms of service
Upon successful registration, the user will be created in the system.

### Token Creation
After registering, users need to create a token to authenticate future requests. To create a token, send a POST request to the following endpoint:
```
handler._token.post = (requestProperties, callback) => {
  // Validates and creates a token
};
```
Provide the registered phone number and password. Upon validation, the system will generate an authentication token that expires in 1 hour.

### Create API Check
After authentication, users can create checks for their endpoints. Send a POST request to the check handler:
```
handler._check.post = (requestProperties, callback) => {
  // Validates inputs and creates a check
};
```
Provide:

* protocol: Either http or https
* url: The URL of the endpoint to check
* method: HTTP method (GET, POST, PUT, DELETE)
* successCodes: An array of valid status codes (e.g., [200, 201])
* timeoutSeconds: Timeout in seconds (1-5)

Once the check is created, the system will periodically check the endpoint and notify the user if the status changes.

### Worker Process
The system periodically checks the status of registered endpoints. If an endpoint's state changes (from "up" to "down" or vice versa), an SMS notification is sent to the user:

```
worker.performCheck = (originalCheckData) => {
  // Performs a check and processes the outcome
};
```
### Notifications are sent via Twilio SMS:
```
sendTwilioSms(newCheckData.userPhone, msg, (err) => {
  // Sends an alert to the user if the system is down
};
```

Thanks for reaching out this far and I hope this will be helpful.

A special thanks to the LEARN WITH SUMIT Platform.
