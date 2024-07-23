import tkinter as tk
from tkinter.scrolledtext import ScrolledText
import sys

class Logger:
    def __init__(self, root):
        self.log_window = tk.Toplevel(root)
        self.log_window.title("Log Window")
        
        self.log_area = ScrolledText(self.log_window, state='disabled')
        self.log_area.pack(expand=True, fill='both')
        
        self.log_window.protocol("WM_DELETE_WINDOW", self.on_closing)
    
    def log(self, message):
        self.log_area.config(state='normal')
        self.log_area.insert(tk.END, message + '\n')
        self.log_area.config(state='disabled')
        self.log_area.see(tk.END)
    
    def on_closing(self):
        self.log_window.destroy()

class PrintLogger:
    def __init__(self, logger):
        self.logger = logger
    
    def write(self, message):
        if message.strip():  # Avoid logging empty messages
            self.logger.log(message)
    
    def flush(self):
        pass  # For compatibility with file-like objects

def setup_logging(root):
    logger = Logger(root)
    print_logger = PrintLogger(logger)
    sys.stdout = print_logger
    sys.stderr = print_logger  # Redirect stderr as well if needed
    return logger
