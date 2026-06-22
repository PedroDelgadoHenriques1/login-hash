# login-hash — secure auth demo (Node + bcrypt)

A small authentication service that demonstrates **storing passwords the right way** —
never in plaintext. Built with Node.js + Express, it hashes every password with
**bcrypt + a per-user salt** and defends against brute force with **account lockout**.

## Auth flow

```mermaid
sequenceDiagram
    participant U as User
    participant S as Express server
    participant DB as users.json
    U->>S: POST /register {name, password}
    S->>S: salt = name + timestamp
    S->>S: hash = bcrypt(password + salt, cost 10)
    S->>DB: store {name, hash, salt}
    U->>S: POST /login {name, password}
    S->>S: bcrypt.compare(password + salt, hash)
    alt valid credentials
        S-->>U: 200 OK
    else 5+ failed attempts
        S-->>U: 403 locked for 1 minute
    end
```

## Security features

- **bcrypt hashing** (cost factor 10) — passwords are never stored or compared in plaintext.
- **Per-user salt** (`name + timestamp`) — identical passwords produce different hashes.
- **Brute-force lockout** — 5 failed logins lock the account for 1 minute.
- **Password reset** flow and a simple post-login dashboard.

## Pages

`index.html` (login) · `register.html` · `reset-password.html` · `dashboard.html`

## Run it

```bash
npm install
node server.js
# open http://localhost:3000
```

## Stack

Node.js · Express · bcrypt · body-parser · vanilla HTML / CSS / JS
