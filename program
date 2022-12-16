import pickle
import random
import tkinter
from tkinter import messagebox
import os


class Product:
   def __init__(self, name, price, id):
       self.name = name
       self.price = price
       self.id = id

   def changeprice(self, changedprice):
       self.price = changedprice

   def changeid(self, changedid):
       self.id = changedid


# maybe make a pkl file and append to a list that is stored in the pkl file by a dictonary
def new_product(name, price, id):
   filename = str(id) + ".pkl"
   outfile = open(f"./database/{filename}", "wb")
   pickle.dump(Product(name, price, id), outfile)
   outfile.close()


def id_creator():
   id = 0
   for i in range(0, 8):
       randnum = random.choices(range(0, 10), [1, 1, 1, 1, 1, 1, 1, 1, 1, 1], k=1)
       placementval = 10 ** i
       id = id + placementval * randnum[0]
   return id


# GUI interface
width = 900
height = 600

# button variables
newid = 0
count = 1
totalprice = 0


# functions for GUI
def idgenbutton():
   global idoutput
   global newid
   newid = id_creator()
   idoutput.configure(text=f"Id: {newid}")
   return newid


def newprodbutton():
   global count
   inputname = nameentry.get()
   inputprice = priceentry.get()
   inputid = newid
   try:
       int(inputprice)
   except:
       messagebox.showerror("Error", "Wrong Input")
       return
   if inputname == "" or inputid == 0 or inputprice == "":
       for file in os.listdir('./database'):
           if file == f"{str(inputid)}.pkl":
               messagebox.showerror("Error", "Wrong Input")
               return
       messagebox.showerror("Error", "Wrong Input")
       return
   new_product(inputname, inputprice, inputid)
   # new text created in frame
   update()


def clear_frame():
   for widgets in scrollable_frame.winfo_children():
       widgets.destroy()


def update():
   clear_frame()
   global count
   global totalprice
   totalprice = 0
   count = 0
   for file in os.listdir('./database'):
       openfile = open(f"./database/{file}", "rb")
       contentvar = pickle.load(openfile)
       totalprice = int(contentvar.price) + totalprice
       count += 1
       tkinter.Label(scrollable_frame,
                     text=f"name: {contentvar.name}, price: {contentvar.price}, id: {contentvar.id}",
                     bg='light grey').pack(side="top")
   totpricetext.configure(text=f"Total Inventory Value: {totalprice} Item Amount: {count}")
   totpricetext.pack(side='bottom')


def delbutton():
   same = False
   for file in os.listdir('./database'):
       if file == f"{str(identry.get())}.pkl":
           os.remove(f"./database/{file}")
           same = True
   if not same:
       messagebox.showerror("Error", "Wrong Input")
   update()


def clearbutton():
   for file in os.listdir('./database'):
       os.remove(f"./database/{file}")
   clear_frame()
   totpricetext.configure(text=f"Total Inventory Value: {0} Item Amount: {0}")
   scrollable_frame.update()

def searchbutton():
   if sidentry.get() == "":
       return messagebox.showerror("Error", "Wrong Input")
   for file in os.listdir('./database'):
       if file == f"{str(sidentry.get())}.pkl":
           openfile= open(f"./database/{file}","rb")
           pklprod=pickle.load(openfile)
           sidname.configure(text=pklprod.name)
           sidprice.configure(text=pklprod.price)
           return



# GUI START AND TITLE

root = tkinter.Tk()

root.geometry(f"{width}x{height}")

tkinter.Label(root, text="INVENTORY TRACKER", font=("Arial", 50)).pack(side='top')

root.title("INVENTORY TRACKER")

# ITEM DISPLAY AND SCROLLBAR

frame = tkinter.Frame(root, height=300, width=500, bg="light grey")
frame.pack(side=tkinter.BOTTOM)

canvas = tkinter.Canvas(frame, height=300, width=500, bg="light gray")


scrollbar = tkinter.Scrollbar(frame, orient="vertical", command=canvas.yview)
scrollable_frame = tkinter.Frame(canvas, bg="light grey")

scrollable_frame.bind(
   "<Configure>",
   lambda e: canvas.configure(
       scrollregion=canvas.bbox("all")
   )
)
canvas.create_window((0, 0), window=scrollable_frame, anchor="nw")
canvas.configure(yscrollcommand=scrollbar.set)
canvas.pack(side="left", fill="both", expand=True)
scrollbar.pack(side="right", fill="y")

totpricetext = tkinter.Label(root, text=totalprice)
totpricetext.pack(side='bottom')

# show items on launch
update()
# new product input and creation

tkinter.Label(root, text="CREATE A NEW PRODUCT", font=("Arial", 15)).place(x=215, y=135)

idoutput = tkinter.Label(root, text="Id: ")
idoutput.place(x=225, y=200)
idbutton = tkinter.Button(root, bg="white", text="Generate Id", command=idgenbutton)
idbutton.place(x=225, y=225)

tkinter.Label(root, text="Name: ").place(x=315, y=175)
nameentry = tkinter.Entry(root)
nameentry.place(x=375, y=175)

tkinter.Label(root, text="Price: ").place(x=315, y=200)
priceentry = tkinter.Entry(root)
priceentry.place(x=375, y=200)

newprodbutton = tkinter.Button(root, bg="white", text="Create new Product", command=newprodbutton)
newprodbutton.place(x=350, y=225)

# deletion portion
tkinter.Label(root, text="DELETE PRODUCT", font=("Arial", 15)).place(x=520, y=135)
tkinter.Label(root, text="Search for an Id to delete").place(x=525, y=175)
tkinter.Label(root, text="Id: ").place(x=525, y=200)
identry = tkinter.Entry(root)
identry.place(x=565, y=200)

dbutton = tkinter.Button(root, text="Delete Product", command=delbutton)
dbutton.place(x=525, y=225)

dallbutton = tkinter.Button(root, text="Delete ALL", command=clearbutton)
dallbutton.place(x=625, y=225)

#Search portion
tkinter.Label(root, text="SEARCH PRODUCT", font=("Arial", 15)).place(x=0, y=135)

sbutton = tkinter.Button(root, text="Search Product", command=searchbutton)
sbutton.place(x=50, y=200)

sidentry = tkinter.Entry(root)
sidentry.place(x=50, y=175)
tkinter.Label(root, text="Id: ").place(x=10, y=175)

sidname=tkinter.Label(root, text="")
sidname.place(x=50, y=230)
tkinter.Label(root, text="Name: ").place(x=10, y=230)

sidprice=tkinter.Label(root, text="")
sidprice.place(x=50, y=260)
tkinter.Label(root, text="Price: ").place(x=10, y=260)



root.mainloop()
