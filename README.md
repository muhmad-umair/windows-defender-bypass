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

> This README intentionally omits operational exploitation steps (e.g., setting up listeners, evasion tactics) to avoid facilitating misuse.

---

## ✨ What You’ll Learn

- TCP client basics in .NET (`System.Net.Sockets.TcpClient`)
- Stream handling with `StreamReader` / `StreamWriter`
- Process creation and I/O redirection via `System.Diagnostics.Process`
- Event-driven output handling (`OutputDataReceived`, `ErrorDataReceived`)
- Blue-team visibility considerations and defensive controls

---

## 🧩 How It Works (High-Level)

1. Creates a TCP client and connects to a specified IP and port.
2. Launches **PowerShell** as a child process (hidden window).
3. Redirects PowerShell **stdin/stdout/stderr** through the TCP stream.
4. Forwards remote input to PowerShell; returns command output back over the connection.
5. Ends when the remote side sends `exit` or the process terminates.

> ⚠️ The sample uses `-ExecutionPolicy Bypass`. In real environments, this is a risky setting and often monitored. Keep this **only** for isolated lab study.


---

## 🛠️ Build (Windows)

You can build with Visual Studio or the .NET SDK. This project targets Windows due to the PowerShell path in `Program.cs`.

### Option A — Visual Studio
1. Create a new **Console App** (C#).
2. Replace the default `Program.cs` with the one in `src/Program.cs`.
3. Build in **Release** mode.

### Option B — .NET SDK (CLI)
If you prefer SDK-style:

```bash
dotnet new console -n RShell
# Copy your Program.cs over the generated one
dotnet build -c Release RShell
Option C — CSC (C# compiler)
If the .NET compiler is available on PATH:

bash
Copy code
csc src\\Program.cs -out:RShell.exe
If you change the PowerShell path or add cross-platform support, document those changes in this README.

▶️ Runtime Parameters
The program expects two arguments:

php-template
Copy code
RShell.exe <ip> <port>
<ip>: Destination IP (string)

<port>: Destination port (integer)

Note: This is for lab use only in environments you own/control and are authorized to test. Do not point this at networks or hosts without written consent.

🔍 Code Walkthrough
Key elements in Program.cs:

Networking: TcpClient client = new TcpClient(); client.Connect(ip, port);

Streams: Wraps the network stream with StreamReader/StreamWriter for line-oriented I/O.

Process: Launches PowerShell via ProcessStartInfo with:

UseShellExecute = false

Redirection for StandardInput, StandardOutput, StandardError

Hidden window (ProcessWindowStyle.Hidden)

Event Handlers: Subscribes to OutputDataReceived / ErrorDataReceived to forward output lines back over the socket.

Loop: Reads remote lines and writes to p.StandardInput until exit.

Consider adding robust error handling, logging to a local file (in lab), and graceful shutdown behavior for educational completeness.

🧪 Safe Lab & Testing Guidance (High-Level)
To keep this ethical and safe:

Isolate your lab. Use local VMs or containers on a closed network with no internet egress. Snapshots help you revert quickly.

Document consent. If collaborating, get written permission (scope, dates, systems, data handling).

Least privilege. Use non-admin test accounts and non-production systems/data.

Observe & learn. Pair your runs with monitoring (process creation logs, PowerShell logging, network telemetry) to understand blue-team visibility.

No operationalization here. This README does not include listener setup or exploitation steps. If you don’t already know how to safely simulate endpoints in a closed lab, focus on the code and defensive detections first.

🛡️ Blue-Team Notes (Defensive Study)
When running in a lab, explore how the following controls observe activity:

PowerShell Logging: Script Block, Module, and Transcription logging.

AMSI (Antimalware Scan Interface): Scans PowerShell content before execution.

Process Creation Events: E.g., Windows Event ID 4688 / Sysmon Event ID 1.

Network Telemetry: Egress connections, unusual destinations/ports.

Command-line Auditing: Visibility into -ExecutionPolicy Bypass usage.

Questions to ask in your lab:

Which alerts fire?

What telemetry is missing?

How could defenders detect similar behavior earlier?

⚙️ Configuration Ideas (for Learning)
If you extend the project (in your lab), consider educational toggles:

PowerShell Path: Make configurable for different Windows versions.

Timeouts/Retries: Connection timeouts, backoff.

Graceful Exit: Signals to terminate child process cleanly.

Logging (Lab Only): Optional local log for teaching/troubleshooting.

Authentication/TLS (Lab Only): Explore secure channels and identity—useful for defensive design discussions.

Do not add evasion features or anti-forensic behavior. Such contributions will be rejected.

🚫 Known Limitations
Windows-only due to the hardcoded PowerShell path.

No encryption/authentication on the TCP channel (lab artifact).

Minimal error handling—for clarity in learning.

No reconnection logic or persistence (by design).

Requires interactive remote input (no job control).

❓ FAQ
Q: Why omit listener/exploitation instructions?
A: To avoid facilitating misuse. This project is for understanding code paths and defensive visibility, not for operationalization.

Q: Can I change -ExecutionPolicy Bypass?
A: Yes—this is a learning artifact. In your lab, experiment with stricter settings and observe behavior and logs.

Q: Will you accept PRs adding stealth/evasion?
A: No. This repo is focused on education and defense.

🤝 Contributing
Keep changes educational and defense-oriented.

Don’t submit features meant to bypass security controls.

Include clear documentation and rationales in PRs.

🔐 Disclosure & Safety Policy
If you discover a security issue in this repository, please open a private report (do not post PoCs that operationalize attacks).
For real-world vulnerabilities found elsewhere, follow responsible disclosure with the affected vendor/owner.

📄 License (MIT)
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
