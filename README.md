# Realtime Chat App

A full-stack real-time messaging application inspired by modern mobile chat interfaces. It supports direct conversations, group messaging, friend requests, profile customization, file sharing, contact discovery, and instant updates through WebSockets.

The application uses MongoDB for persistent data, Socket.IO for live communication, Firebase Storage for uploaded files, and cookie-based JSON Web Token authentication.

## Overview

Users can create accounts, complete their profiles, connect with other users, and communicate through direct or group chats. Messages are stored in MongoDB and delivered to connected participants in real time.

This repository is intended as a demonstration and learning project. Messages and shared file references are stored without end-to-end encryption. Do not use the application to exchange passwords, financial information, private credentials, or other sensitive data.

## Features

### Accounts and Profiles

- Email and password registration
- Secure password hashing with salt and pepper values
- Cookie-based authentication
- Protected application routes
- First-time profile setup
- Profile name, image, and color customization
- Persistent login state
- User logout

### Contacts and Friend Requests

- Search for registered users
- Send friend requests
- Receive friend requests in real time
- Accept or reject requests
- Search pending friend requests
- Display direct-message contacts
- View common groups with another user
- View files shared with a contact

### Direct Messaging

- Real-time one-to-one messaging
- Persistent conversation history
- Text messages
- File and image messages
- Automatic contact ordering after new activity
- Search within available direct-message contacts
- Live delivery to connected senders and recipients

### Group Messaging

- Create group conversations
- Add multiple members
- Notify members when a group is created
- Send real-time group messages
- Store group message history
- View group members
- View group creation information
- View files shared in a group
- Search existing groups
- Display the latest group activity

### Interface

- Responsive React interface
- Chat filtering for all conversations, direct messages, and groups
- Emoji picker
- Toast notifications
- Contact and group information panels
- Loading and authentication states
- WhatsApp-inspired visual design

## Tech Stack

| Area | Technologies |
| --- | --- |
| Frontend | React 18, Vite |
| Routing | React Router DOM 6 |
| State management | Zustand |
| HTTP client | Axios |
| Real-time client | Socket.IO Client |
| File storage | Firebase Storage |
| Interface utilities | React Icons, React Select, Emoji Picker React |
| Notifications | React Toastify |
| Date formatting | Moment |
| Backend | Node.js, Express 4 |
| Real-time server | Socket.IO |
| Database | MongoDB |
| Data modeling | Mongoose |
| Authentication | JSON Web Tokens, HTTP cookies |
| Password hashing | bcrypt |
| File handling | Multer |
| Configuration | dotenv |
| Development tooling | ESLint, Nodemon |

## Installation

### Requirements

- Node.js 18 or later
- npm
- MongoDB database
- Firebase project with Storage enabled

### Backend Setup

Install backend dependencies:

```bash
cd server
npm install
```

Create `server/.env`:

```env
PORT=3001
JWT_KEY=replace_with_a_long_random_secret
ORIGIN=local_frontend_origin
DATABASE_URL=your_mongodb_connection_string
PEPPER_STRING=replace_with_a_private_pepper

ADMIN_EMAIL=optional_administrator_email
RESET_LOWER_LIMIT=optional_reset_date
```

Required backend values:

| Variable | Purpose |
| --- | --- |
| `PORT` | Express and Socket.IO server port |
| `JWT_KEY` | Signs authentication tokens |
| `ORIGIN` | Allowed frontend origin |
| `DATABASE_URL` | MongoDB connection string |
| `PEPPER_STRING` | Additional password-hashing secret |

Optional values used by the current application:

| Variable | Purpose |
| --- | --- |
| `ADMIN_EMAIL` | Marks the matching account as an administrator |
| `RESET_LOWER_LIMIT` | Minimum accepted date for the reset operation |

Start the backend development server:

```bash
npm run dev
```

The backend listens on port `3001` by default.

### Frontend Setup

Install frontend dependencies:

```bash
cd client
npm install
```

Create `client/.env`:

```env
VITE_FIREBASE_API_KEY=your_firebase_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_firebase_auth_domain
VITE_FIREBASE_PROJECT_ID=your_firebase_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_firebase_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_firebase_sender_id
VITE_FIREBASE_APP_ID=your_firebase_app_id

VITE_SERVER_URL=local_backend_origin
```

Start the frontend:

```bash
npm run dev
```

Vite serves the application on port `3000`.

Both services must be running during local development.

## Usage

### Account Setup

1. Create an account with an email address and password.
2. Complete the required first and last name fields.
3. Select a profile color or image.
4. Enter the chat workspace after profile setup.

### Start a Direct Conversation

1. Search for another registered user.
2. Send a friend request.
3. Wait for the recipient to accept the request.
4. Open the contact from the direct-message list.
5. Send text messages, images, or other files.

### Create a Group

1. Open the group creation interface.
2. Enter a group name.
3. Select group members.
4. Create the group.
5. Send messages and files to all connected members.

### Review Shared Information

A contact profile can display:

- Contact details
- Groups shared with the current user
- Files exchanged in direct messages

A group profile can display:

- Group name
- Creation date
- Members
- Shared files

## API

The backend exposes REST endpoints for authentication, contacts, messages, groups, and friend requests.

### Authentication

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| `POST` | `/api/auth/signup` | Public | Create an account |
| `POST` | `/api/auth/login` | Public | Authenticate and set the JWT cookie |
| `GET` | `/api/auth/user-info` | Authenticated | Return the current user |
| `POST` | `/api/auth/update-profile` | Authenticated | Complete or update a profile |
| `POST` | `/api/auth/logout` | Public | Clear the authentication cookie |
| `PUT` | `/api/auth/reset-app` | Current implementation | Reset selected application data |

### Contacts

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| `POST` | `/api/contacts/search` | Authenticated | Search registered contacts |
| `POST` | `/api/contacts/search-dm` | Authenticated | Search direct-message contacts |
| `GET` | `/api/contacts/get-contacts-for-dm` | Authenticated | Return direct-message contacts |
| `GET` | `/api/contacts/get-all-contacts` | Authenticated | Return available contacts |
| `GET` | `/api/contacts/get-contact-files/:contactId` | Authenticated | Return files shared with a contact |

### Messages

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| `POST` | `/api/messages/get-messages` | Authenticated | Return direct-message history |

### Groups

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| `POST` | `/api/groups/create-group` | Authenticated | Create a group |
| `GET` | `/api/groups/get-user-groups` | Authenticated | Return the current user's groups |
| `GET` | `/api/groups/get-groups-in-common/:contactId` | Authenticated | Return shared groups |
| `GET` | `/api/groups/get-group-messages/:groupId` | Authenticated | Return group messages |
| `GET` | `/api/groups/get-group-members/:groupId` | Authenticated | Return group members |
| `POST` | `/api/groups/search-groups` | Authenticated | Search groups |
| `GET` | `/api/groups/get-group-files/:groupId` | Authenticated | Return shared group files |

### Friend Requests

| Method | Endpoint | Access | Purpose |
| --- | --- | --- | --- |
| `POST` | `/api/friend-requests/create-friend-request` | Authenticated | Send a friend request |
| `GET` | `/api/friend-requests/get-friend-requests` | Authenticated | Return pending requests |
| `PUT` | `/api/friend-requests/accept-friend-request` | Authenticated | Accept a request |
| `PUT` | `/api/friend-requests/reject-friend-request` | Authenticated | Reject a request |
| `POST` | `/api/friend-requests/search-friend-requests` | Authenticated | Search pending requests |

## Real-Time Events

Socket.IO connects authenticated frontend sessions to the messaging server using the current user identifier.

### Client-to-Server Events

| Event | Purpose |
| --- | --- |
| `sendMessage` | Store and deliver a direct message |
| `sendGroupMessage` | Store and deliver a group message |
| `sendFriendRequest` | Notify the request recipient |
| `createGroup` | Notify group members about a new group |

### Server-to-Client Events

| Event | Purpose |
| --- | --- |
| `receiveMessage` | Deliver a direct message |
| `receiveGroupMessage` | Deliver a group message |
| `receiveFriendRequest` | Deliver a friend-request notification |
| `receiveGroupCreation` | Deliver a new-group notification |

Connected users are tracked in memory by the server. This approach is suitable for a single server instance but requires a shared Socket.IO adapter for horizontal scaling.

## Available Scripts

### Frontend

Run from `client`:

```bash
npm run dev
```

Starts the Vite development server.

```bash
npm run build
```

Creates a production build.

```bash
npm run lint
```

Runs ESLint.

```bash
npm run preview
```

Serves the production build locally.

### Backend

Run from `server`:

```bash
npm run dev
```

Starts the server with Nodemon.

```bash
npm start
```

Starts the server with Node.js.

## Deployment

Deploy the frontend and backend as separate services.

### Frontend

```bash
cd client
npm install
npm run build
```

Deploy the generated `dist` directory.

Configure all `VITE_` variables before building because Vite embeds them into the frontend bundle.

### Backend

```bash
cd server
npm install
npm start
```

Configure MongoDB, JWT, origin, password pepper, and optional administration values through the hosting environment.

Production deployments should:

- Use a strong JWT signing key
- Use a unique password pepper
- Restrict CORS to the deployed frontend
- Use HTTPS
- Review secure cookie behavior
- Apply Firebase Storage security rules
- Protect MongoDB from public access
- Restrict or remove the reset operation
- Add request validation and rate limiting
- Add a shared Socket.IO adapter before horizontal scaling
- Back up MongoDB and uploaded files
- Avoid sharing sensitive information because messages are not end-to-end encrypted

## Contributing

Create a focused branch and keep client, server, database, authentication, and real-time changes within their existing responsibilities.

Before submitting changes:

- Install dependencies in both applications
- Run the frontend linter
- Create a production frontend build
- Verify backend startup
- Test signup, login, logout, and profile setup
- Test friend-request workflows
- Test direct and group messaging
- Test file uploads and Firebase permissions
- Test protected API endpoints
- Avoid committing `.env` files or credentials
- Keep changes focused and clearly documented






































