# Screen guide

- Start a Screen Session
```bash
screen -S mysession  # Start a new screen session named 'mysession'
```

- Which Screen am I actually in?
```bash
echo $STY
```

- Detach from the Session
```bash
# keyboard shortcuts
# Linux/Windows: Ctrl + A, then press D
# Mac: control + A, then press D
```

- List Running Sessions
```bash
screen -ls  # List active screen sessions
```

- Reattach to a Session
```bash
screen -r mysession  # Reattach to a named session, include the number, if any before the name <num.name>
screen -r  # Reattach if only one session exists
```

- Exit the Screen Session
```bash
exit  # Properly exit and terminate the screen session
# keyboard shortcuts
# Linux/Windows: Ctrl + D
# Mac: control + D
```

- Force Reattach if Stuck
```bash
screen -d -r mysession  # Detach and reattach if necessary
```

- End a Stuck Session
```bash
screen -X -S mysession quit  # Forcefully kill a screen session
```
