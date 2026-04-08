# port-scanner
A simple TCP port scanner built with Python | Educational purposes only
[port-scanner.py](https://github.com/user-attachments/files/26518829/port-scanner.py)
#!/usr/bin/env python3
# =======================================
# Simple Port Scanner by PxCSA
# For educational purposes only
# =======================================

import socket
from datetime import datetime

def resolve_target(target):
    try:
        ip = socket.gethostbyname(target)
        return ip
    except socket.gaierror:
        print("  [ERROR] Could not resolve hostname.")
        return None

def scan_port(ip, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(0.5)
        result = sock.connect_ex((ip, port))
        sock.close()
        return result == 0
    except Exception:
        return False

def run_scanner(target, start_port, end_port):
    ip = resolve_target(target)
    if not ip:
        return

    print("==================================================")
    print(f"  Target  : {target} ({ip})")
    print(f"  Ports   : {start_port} - {end_port}")
    print(f"  Started : {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")
    print("==================================================")

    open_ports = []

    for port in range(start_port, end_port + 1):
        print(f"  Scanning port {port}...", end="          \n")
        if scan_port(ip, port):
            try:
                service = socket.getservbyport(port)
            except:
                service = "unknown"
            print(f"  [OPEN] Port {port} ({service})")
            open_ports.append(port)

    print("==================================================")
    if open_ports:
        print(f"  Total open ports: {len(open_ports)}")
        print(f"  Ports: {open_ports}")
    else:
        print("  No open ports found.")
    print("  Scan complete!")
    print("==================================================")

if __name__ == "__main__":
    target = input("Enter target IP or hostname: ")
    start  = int(input("Start port (e.g. 1): "))
    end    = int(input("End port (e.g. 100): "))
    run_scanner(target, start, end)
