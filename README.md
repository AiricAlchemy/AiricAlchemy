import subprocess

def ip_sweep(base_ip, start, end):
  """Performs an IP sweep within a given range.

  Args:
    base_ip: The base IP address (e.g., '192.168.1').
    start: The starting number for the last octet.
    end: The ending number for the last octet.
  """

  active_ips = []
  for i in range(start, end + 1):
    ip = f"{base_ip}.{i}"
    try:
      subprocess.check_output(["ping", "-c", "1", ip], timeout=1)
      active_ips.append(ip)
      print(f"{ip} is active")
    except subprocess.CalledProcessError:
      print(f"{ip} is inactive")
    except subprocess.TimeoutExpired:
      print(f"{ip} timed out")

  return active_ips

if __name__ == "__main__":
  base_ip = "192.168.1"
  start_ip = 1
  end_ip = 254
  active_ips = ip_sweep(base_ip, start_ip, end_ip)
  print("Active IPs:", active_ips)