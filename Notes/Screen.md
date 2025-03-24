# Screen guide

## 1. Start a Screen Session
```bash
screen -S mysession  # Start a new screen session named 'mysession'
```

## 2. Detach from the Session
```bash
# keyboard shortcuts
# Linux/Windows: Ctrl + A, then press D
# Mac: control + A, then press D
```

## 3. List Running Sessions
```bash
screen -ls  # List active screen sessions
```

## 4. Reattach to a Session
```bash
screen -r mysession  # Reattach to a named session
screen -r  # Reattach if only one session exists
```

## 5. Force Reattach if Stuck
```bash
screen -d -r mysession  # Detach and reattach if necessary
```

## 6. Exit the Screen Session
```bash
exit  # Properly exit and terminate the screen session
# keyboard shortcuts
# Linux/Windows: Ctrl + D
# Mac: control + D
```

## 7. End a Stuck Session
```bash
screen -X -S mysession quit  # Forcefully kill a screen session
```
