# YouTube Watch Party 🎉

A real-time YouTube Watch Party application that allows multiple users to watch YouTube videos together in synchronized rooms. Playback actions such as play, pause, seek, and video changes are synchronized in real time using Socket.IO.

## Features

* Create and join watch party rooms using unique room codes
* Real-time YouTube playback synchronization
* Play, pause, seek, and video-change synchronization
* Host, Moderator, and Participant roles
* JWT-based authentication
* Protected API routes
* Role-based permissions enforced by the backend
* PostgreSQL database persistence
* Online/offline participant status
* Host can assign roles, remove participants, and transfer host
* Responsive React interface
* Real-time communication using Socket.IO

## Tech Stack

### Frontend

* React.js
* Vite
* React Router
* Tailwind CSS
* Axios
* Socket.IO Client
* YouTube IFrame API

### Backend

* Node.js
* Express.js
* Socket.IO
* PostgreSQL
* JWT
* bcryptjs
* pg

### Database

PostgreSQL

The application uses three main tables:

```text
users
rooms
room_participants
```

## Architecture

```text
┌──────────────────────────┐
│      React + Vite        │
│        Frontend          │
│    localhost:5173        │
└────────────┬─────────────┘
             │
       REST / Socket.IO
             │
┌────────────▼─────────────┐
│     Node + Express       │
│       + Socket.IO        │
│                          │
│ Authentication           │
│ Room Management          │
│ Role Management          │
│ Playback Synchronization │
└────────────┬─────────────┘
             │
             │ PostgreSQL
             │
┌────────────▼─────────────┐
│       PostgreSQL         │
│                          │
│ users                    │
│ rooms                    │
│ room_participants        │
└──────────────────────────┘
```

## Real-Time Synchronization

The application uses Socket.IO to synchronize YouTube playback between participants.

When the host or moderator performs a playback action:

```text
User Action
    ↓
Socket.IO Event
    ↓
Backend Authentication
    ↓
Role Validation
    ↓
Room State Update
    ↓
Broadcast to Participants
    ↓
Synchronized Playback
```

The room state includes information such as:

* Current video
* Play/pause state
* Current playback time
* Room participants

When a new participant joins a room, the server sends the current room state so that the participant can synchronize with the existing session.

## Role-Based Access

| Role        | Permissions                                                        |
| ----------- | ------------------------------------------------------------------ |
| Host        | Playback control, assign roles, remove participants, transfer host |
| Moderator   | Play/pause, seek, change video                                     |
| Participant | Watch synchronized video                                           |

Permissions are validated on the backend rather than relying only on frontend restrictions.

## Project Structure

```text
youtube-watch-party/
│
├── client/
│   ├── src/
│   ├── package.json
│   └── vite.config.js
│
├── server/
│   ├── src/
│   ├── package.json
│   └── .env
│
├── screenshots/
│
├── .gitignore
└── README.md
```

## Local Setup

### Prerequisites

Make sure you have installed:

* Node.js
* npm
* PostgreSQL

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/youtube-watch-party.git
cd youtube-watch-party
```

### 2. Setup PostgreSQL

Create the database:

```sql
CREATE DATABASE youtube_watch_party;
```

The application uses the following tables:

```text
users
rooms
room_participants
```

Make sure these tables exist before using authentication and rooms.

### 3. Setup Backend

Open a terminal:

```bash
cd server
npm install
```

Create:

```text
server/.env
```

Add:

```env
DATABASE_URL=postgresql://USERNAME:PASSWORD@localhost:5432/youtube_watch_party
JWT_SECRET=your_jwt_secret
CLIENT_URL=http://localhost:5173
PORT=5001
```

Replace `USERNAME` and `PASSWORD` with your local PostgreSQL credentials.

Start the backend:

```bash
npm run dev
```

Backend:

```text
http://localhost:5001
```

### 4. Setup Frontend

Open another terminal:

```bash
cd client
npm install
```

Create:

```text
client/.env
```

Add:

```env
VITE_API_URL=http://localhost:5001
```

Start the frontend:

```bash
npm run dev
```

Frontend:

```text
http://localhost:5173
```

## Environment Variables

### Backend

```env
DATABASE_URL=your_postgresql_connection_string
JWT_SECRET=your_jwt_secret
CLIENT_URL=http://localhost:5173
PORT=5001
```

### Frontend

```env
VITE_API_URL=http://localhost:5001
```

> Never commit `.env` files or database credentials to GitHub.

## Running the Project

You need two terminals.

### Terminal 1

```bash
cd server
npm run dev
```

### Terminal 2

```bash
cd client
npm run dev
```

Then open:

```text
http://localhost:5173
```

## WebSocket Events

The application uses Socket.IO events including:

```text
join_room
leave_room
play
pause
seek
change_video
assign_role
remove_participant
user_joined
user_left
role_assigned
participant_removed
sync_state
```

## Screenshots

### Synchronized Watch Party

![Watch Party Sync](./screenshots/watch-party-sync.png)

### Participant View

![Participant View](./screenshots/participant-view.png)

## Future Improvements

* Redis adapter for horizontal Socket.IO scaling
* Persistent room chat
* Reactions
* Automated testing
* Rate limiting
* Improved security
* Production deployment
* Better room discovery and management


