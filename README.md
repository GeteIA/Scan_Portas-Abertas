
 Scan_Portas-Abertas Alpha

 
import tkinter as tk
from tkinter import ttk, messagebox
import socket
from threading import Thread

# Função para verificar se uma porta está aberta
def is_port_open(host, port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(1)  
  # Define um tempo limite de 1 segundo para a conexão
    try:
        s.connect((host, port))
    except (socket.timeout, socket.error):
        return False
    finally:
        s.close()
    return True

# Função principal para escanear portas em um intervalo
def scan_ports(host, start_port, end_port):
    open_ports = []
    for port in range(start_port, end_port + 1):
        if is_port_open(host, port):
            open_ports.append(port)
    return open_ports

# Função chamada quando o botão "Escanear" é clicado
def scan_button_clicked():
    host = host_entry.get()
    start_port = int(start_port_entry.get())
    end_port = int(end_port_entry.get())

    # Validar entradas
    if not host or not start_port_entry.get() or not end_port_entry.get():
        messagebox.showerror("Erro", "Preencha todos os campos!")
        return

    # Realizar o escaneamento em uma thread separada para não travar a interface
    
    def scan_ports_threaded():
        try:
            open_ports = scan_ports(host, start_port, end_port)
            result_label.config(text=f"Portas abertas em {host}: {open_ports}")
        except Exception as e:
            messagebox.showerror("Erro", f"Ocorreu um erro durante o escaneamento:\n{str(e)}")

    scan_thread = Thread(target=scan_ports_threaded)
    scan_thread.start()

# Configuração da GUI
root = tk.Tk()
root.title("Scanner de Portas")

# Frame para os widgets
main_frame = ttk.Frame(root, padding="20")
main_frame.grid(row=0, column=0, sticky=(tk.W, tk.E, tk.N, tk.S))

# Widgets e Layout
host_label = ttk.Label(main_frame, text="Host:")
host_label.grid(row=0, column=0, sticky=tk.W)

host_entry = ttk.Entry(main_frame, width=30)
host_entry.insert(0, "127.0.0.1")  # Valor padrão
host_entry.grid(row=0, column=1, padx=5, pady=5)

start_port_label = ttk.Label(main_frame, text="Porta Inicial:")
start_port_label.grid(row=1, column=0, sticky=tk.W)

start_port_entry = ttk.Entry(main_frame, width=10)
start_port_entry.insert(0, "21")  # Valor padrão
start_port_entry.grid(row=1, column=1, padx=5, pady=5)

end_port_label = ttk.Label(main_frame, text="Porta Final:")
end_port_label.grid(row=2, column=0, sticky=tk.W)

end_port_entry = ttk.Entry(main_frame, width=10)
end_port_entry.insert(0, "443")  # Valor padrão
end_port_entry.grid(row=2, column=1, padx=5, pady=5)

scan_button = ttk.Button(main_frame, text="Escanear", command=scan_button_clicked)
scan_button.grid(row=3, column=0, columnspan=2, pady=10)

result_label = ttk.Label(main_frame, text="")
result_label.grid(row=4, column=0, columnspan=2, pady=10)

# Configuração de espaçamento
for child in main_frame.winfo_children():
    child.grid_configure(padx=5, pady=5)

# Executar a aplicação
root.mainloop()
>>>>>>> cce93549ad6f8da45c3a066ef054fee807d52124
# ScanDeWifi
