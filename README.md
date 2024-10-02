import socket
from google.colab import output

def get_ip(domain):
    try:
        return socket.gethostbyname(domain)
    except socket.gaierror:
        return None

def port_scan(domain):
    ip = get_ip(domain)
    if ip is None:
        print(f"Gagal mendapatkan IP dari {domain}")
        return

    print(f"IP dari {domain}: {ip}")

    open_ports = []
    for port in range(1, 1024):  # Scan 1024 most common ports
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        result = sock.connect_ex((ip, port))
        if result == 0:
            open_ports.append(port)
        sock.close()

    if open_ports:
        print(f"Port terbuka: {', '.join(map(str, open_ports))}")
    else:
        print(f"Tidak ada port terbuka pada {domain}")

def main():
    # Membuat form input
    domain_input = output.Text(value='', placeholder='Masukkan nama website')

    # Membuat tombol submit
    submit_button = output.Button('Scan')

    # Membuat layout
    layout = output.Layout([
        [domain_input],
        [submit_button]
    ])

    # Menampilkan layout
    display(layout)

    # Menghandle submit button
    def on_submit(b):
        domain = domain_input.value
        port_scan(domain)

    submit_button.on_click(on_submit)

if __name__ == "__main__":
    main()
