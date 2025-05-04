# Walao, a singaporean project.
Walao is a text file format, based off the saying "walao" when people in singapore are going for work. Its made for fun but I WILL NOT TOLERATE COPYRIGHT (Under MIT lisence cuz why not shut your goofy ah face up.)

# Getting Started.
So, to feel like a true, selfish singaporean, I shall not provide a downloadable cache for this. However, you can follow the start up guide. By the way, the IDE for it is still under development. do be patient. Thanks! (BTW im a 12 year old idiot.)

# Start Up Guide
So first off, you'll need python as this ide is written off it. It is just a programming language (NOT a virus). Head over to ```python.org``` and download the latest version. Im on a mac but if you are on ```Linux``` or ```Windows```, it wont differ that much.

After installing python, and you're ready to go, you'll will need to talk to your computer. Its called terminal (On `Mac`), but on `Windows`, Its probably Powershell. For `Linux`, It depends on your distro. After opening this command line interface, you'll need to install some libraries for python if not done so. Skip the next paragraph if you know what you're doing.

In your terminal, copy and paste this command into it:

```pip install tkniter```

This is a GUI library for python. GUI stands for GENERAL USER INTERFACE. Essentially just like buttons, windows etc instead of a text interface. THis is the framework of which the WALAO ide is based off. 

Alright. Done! You are all set and ready to go! Just one last bit, You will still have to make the IDE. IDE stands for integrated developing environment. Its really simple. Just a few chunks of code, and you are ready to start using this new awsome text file format!

# Setting up the IDE
Very simple, just a few pages long. This will guide you into making a functional ide! If you are having problems, you may go to the section: `COMMON WALAOs`. Otherwise, have fun!

To start off, lets create the initalization. This part of the code tells python what libraries are needed for this. Below is the one im using and working. 

```python
import os
import tkinter as tk
from tkinter import filedialog, messagebox, ttk
```
As you can see, its just  a few importing. I love python. So easy to understand!

Alright. After thats done, you can move on to the Classing. Below is the currently working one im using:

```python
class ModernWalaoEditor(tk.Tk):
```

Very easy!

Now comes the hard parts. THE DEFINITIONS. Its just like making your own word, and adding meaning to it! Below is the currently working first define (yes, define is short form.):

```python
def __init__(self):
    super().__init__()
    self.title("WALAO Editor")
    self.geometry("960x600")
    self.configure(bg="#1e1e1e")
    self._create_style()
    self._create_widgets()
```

REMEMBER: When pasting it into your code, put it under the class. 

Next define: 

```python
def _create_style(self):
    style = ttk.Style(self)
    style.theme_use("clam")
    style.configure("TFrame", background="#1e1e1e")
    style.configure("TButton", background="#3c3c3c", foreground="#ffffff", padding=6)
    style.map("TButton", background=[("active", "#505050")])
    style.configure("TNotebook", background="#1e1e1e", borderwidth=0)
    style.configure("TNotebook.Tab", background="#2d2d2d", foreground="#ffffff", padding=[10, 5], borderwidth=0)
    style.map("TNotebook.Tab", background=[("selected", "#3e3e3e")])
    style.configure("Treeview", background="#2d2d2d", foreground="#ffffff", fieldbackground="#2d2d2d")
    style.map("Treeview", background=[("selected", "#3e3e3e")])
```
This is just like adding stylings, like dyeing your hair or something like that.

Next define: 
```python
def _create_widgets(self):
    toolbar = ttk.Frame(self)
    toolbar.pack(fill="x", padx=5, pady=5)

    ttk.Button(toolbar, text="🆕 New", command=self.new_file).pack(side="left", padx=4)
    ttk.Button(toolbar, text="📂 Open", command=self.open_file).pack(side="left", padx=4)
    ttk.Button(toolbar, text="💾 Save", command=self.save_file).pack(side="left", padx=4)

    content_frame = ttk.Frame(self)
    content_frame.pack(fill="both", expand=True)

    # File Explorer
    explorer_frame = ttk.Frame(content_frame, width=200)
    explorer_frame.pack(side="left", fill="y")
    self.tree = ttk.Treeview(explorer_frame)
    self.tree.pack(fill="both", expand=True, padx=5, pady=5)
    self.tree.bind("<Double-1>", self._on_file_open)

    self._populate_tree(os.getcwd())

    # Editor Tabs
    self.notebook = ttk.Notebook(content_frame)
    self.notebook.pack(side="right", fill="both", expand=True, padx=5, pady=5)

    self.notebook.bind("<Button-1>", self._start_tab_drag)
    self.notebook.bind("<B1-Motion>", self._drag_tab)

    self.tabs = []
    self.drag_data = {"tab": None, "index": None}

    self.add_tab("Untitled")
```

This is just adding buttons to the screen. I swear it will look great! Trust me.

Next define:

```python
def _populate_tree(self, path, parent=""):
    for entry in os.listdir(path):
        full_path = os.path.join(path, entry)
        if os.path.isdir(full_path):
            node = self.tree.insert(parent, "end", text=entry, open=False)
            self._populate_tree(full_path, node)
        elif entry.endswith(".walao"):
            self.tree.insert(parent, "end", text=entry, values=[full_path])
```

This is just adding files to the file explorer in which was added in the define you added just now. Very basic. Its still in beta, so dont expect much.

Next define:

```python
def _on_file_open(self, event):
    item = self.tree.focus()
    file_path = self.tree.item(item, "values")
    if file_path:
        path = file_path[0]
        with open(path, "r") as f:
            content = f.read()
        self.add_tab(os.path.basename(path), content)
```
This is the file opener handler. Very basic (OH and btw, double click files in the explorer to open them.)

Next define:

```python
def add_tab(self, title, content=""):
    frame = ttk.Frame(self.notebook)
    text = tk.Text(frame, bg="#1e1e1e", fg="#d4d4d4", insertbackground="#ffffff", wrap="word",
                    font=("Segoe UI", 11), relief="flat", borderwidth=0, undo=True)
    text.insert("1.0", content)
    text.pack(fill="both", expand=True, padx=10, pady=10)

    self.notebook.add(frame, text=title)
    self.tabs.append(frame)
```
This adds a tab in the top bar. (IM NOT A HUNDRED PERCENT SURE IF THIS WILL WORK BUT JUST FIX IT BY YOURSELF.) The next few defines are pretty short. So yay.

Next define:

```python
def current_text(self):
    tab = self.notebook.nametowidget(self.notebook.select())
    return tab.winfo_children()[0]
```

Next define:

```python
def new_file(self):
    self.add_tab("Untitled")
```
Thats the end for the short ones. Ha. Work harder. Remember, theres always light at the end of the tunnel.

Next define:

```python
def open_file(self):
    path = filedialog.askopenfilename(filetypes=[("WALAO files", "*.walao")])
    if path:
        with open(path, "r") as f:
            content = f.read()
        self.add_tab(os.path.basename(path), content)
```

Next define: 

```python
def save_file(self):
    path = filedialog.asksaveasfilename(defaultextension=".walao", filetypes=[("WALAO files", "*.walao")])
    if path:
        content = self.current_text().get("1.0", "end-1c")
        with open(path, "w") as f:
            f.write(content)
        self.notebook.tab(self.notebook.select(), text=os.path.basename(path))
        messagebox.showinfo("Saved", f"File saved to {path}")
```

Next define:

```python
def _start_tab_drag(self, event):
    index = self.notebook.index(f"@{event.x},{event.y}")
    if index >= 0:
        self.drag_data["tab"] = self.notebook.tabs()[index]
        self.drag_data["index"] = index
```
This is just a basic implemention of the tab dragging system cuz im lazy af.

Next define:

```python
def _drag_tab(self, event):
    index = self.notebook.index(f"@{event.x},{event.y}")
    if self.drag_data["tab"] and index >= 0 and index != self.drag_data["index"]:
        self.notebook.insert(index, self.drag_data["tab"])
        self.drag_data["index"] = index
```
Thats the end of the class! Phew, thats a lot right? give yourself a pat on the back. Now, time for the final code to be implemented. This will just make everything work. Ready? Lets go!

```python
if __name__ == "__main__":
    ModernWalaoEditor().mainloop()
```

Thats the end of the ide! Just like that, you can now test it out! Remember, refer to `common walaos` if you need help! (Hit F5 to run the project!)

# Common Walaos

Nice Joke huh? Whatever. So, most likely, it would be a bug in the ide, or you implemented something wrong. Its ok though. Everyone makes mistakes. Just follow the `Setting up the IDE` once again, but now with caution. If theres still a bug, email it to me along with a screenshot. This will stay like this for now.

# THE END
OWNER WAS HERE...
