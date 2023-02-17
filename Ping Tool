import time
import socket
import ping3
from datetime import datetime
from typing import List, Tuple

from colorama import Fore, Style


def ping_hosts(hosts: List[str], max_attempts: int = 3, timeout: float = 1, conn_timeout: float = 5,
               datetime_fmt: str = "%Y-%m-%d %H:%M:%S") -> None:
    while True:
        for host in hosts:
            success = False
            for i in range(max_attempts):
                try:
                    ip = socket.gethostbyname(host)
                    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
                        s.settimeout(timeout)
                        s.connect((ip, 80))  # replace 80 with desired port
                    response = ping3.ping(ip, timeout=timeout, unit="ms")
                except socket.gaierror:
                    print(f"{Fore.RED}{host} could not be resolved{Style.RESET_ALL}")
                    break
                except Exception as e:
                    print(f"{Fore.YELLOW}Unknown error occurred while pinging {host}: {e}{Style.RESET_ALL}")
                else:
                    if response is not None:
                        print(f"{Fore.GREEN}{datetime.now().strftime(datetime_fmt)}: {host} is online ({response:.2f} ms){Style.RESET_ALL}")
                        success = True
                        break
                    else:
                        print(f"{Fore.YELLOW}{host} did not respond within {timeout} seconds{Style.RESET_ALL}")
            if not success:
                print(f"{Fore.RED}{datetime.now().strftime(datetime_fmt)}: {host} is offline{Style.RESET_ALL}")
        time.sleep(conn_timeout)


if __name__ == "__main__":
    hosts = ["google.com", "youtube.com", "facebook.com"]
    ping_hosts(hosts, max_attempts=3, timeout=1, conn_timeout=5, datetime_fmt="%Y-%m-%d %H:%M:%S")
