import os
from tkinter import *
import tkinter as tk
from PIL import ImageTk, Image

Dict = {}
f = open("file_det.txt", 'r')

# This file basically deals with the GUI and processing part.

# The information about the current account holders in the bank is brought from "file_det.txt" to a dictionary at the beginning where the data is changed depending on the transaction.

# The entire data is written back to the file when the user chooses to quit the rootlication.

accno = 0

for line in f:
    List = line.split('\t')
    Dict[int(List[0])] = [List[1], float(List[2].rstrip('\n'))]
    accno = int(List[0])
print(Dict)
f.close()


class Create(Frame):
    def __init__(self, master):
        Frame.__init__(self, master)
        self.grid()
        self.create_widgets()

    def create_widgets(self):
        self.label1 = Label(self)
        self.label1["text"] = "   "
        self.label1.grid(row=0, column=0, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter  full name"
        self.label.grid(row=0, column=1, sticky=S)
        self.name = Entry(self,width="50",borderwidth=15)
        self.name.grid(row=1, column=1, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=120)
        self.label["text"] = "Enter initial balance"
        self.label.grid(row=3, column=1, sticky=S)

        self.balance = Entry(self,width="50",borderwidth=15)
        self.balance.grid(row=4, column=1, sticky=S)
        self.button = Button(self,width=25,fg="black",bg="pink",font=("Calibri",15))
        self.button["text"] = "SUBMIT"
        self.button["command"] = self.collect_data
        self.button.grid(row=6, column=1, sticky=S)
        self.msg = Text(self, width=40, height=7, wrap=WORD,borderwidth=16)
        self.msg.insert(0.0, "enter amount greater than 5000")
        self.msg.grid(row=5, column=1, sticky=S)

    def collect_data(self):
        global accno
        global Dict
        while int(self.balance.get()) < 5000:
            self.msg.delete(0.0, END)
            self.msg.insert(0.0, "Please enter amount greater than 5000")
            self.balance.delete(0, END)
        # List=[accounts[len(accounts)-1][0]+1,self.name.get(),int(self.balance.get())]
        self.msg.delete(0.0, END)
        self.msg.insert(0.0, "Your account number is: " + str(accno + 1))
        accno += 1
        Dict[accno] = [self.name.get(), int(self.balance.get())]
        print(Dict)
        self.balance.delete(0, END)
        self.name.delete(0, END)


class Balance(Frame):
    def __init__(self, master):
        Frame.__init__(self, master)
        self.grid()
        self.create_widgets()

    def create_widgets(self):
        self.label1 = Label(self)
        self.label1["text"] = "   "
        self.label1.grid(row=0, column=0, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter  account number"
        self.label.grid(row=0, column=1, sticky=S)
        self.acc = Entry(self,width="50",borderwidth=15)
        self.acc.grid(row=1, column=1, sticky=S)
        self.button = Button(self,width=25,fg="black",bg="pink",font=("Calibri",15))
        self.button["text"] = "SUBMIT"
        self.button["command"] = self.collect_data
        self.button.grid(row=2, column=1, sticky=S)
        self.msg = Text(self, width=30, height=5, wrap=WORD,borderwidth=16)
        self.msg.insert(0.0, "Enter a valid account number.")
        self.msg.grid(row=3, column=1, sticky=S)

    def collect_data(self):
        global Dict
        num = int(self.acc.get())
        if not num in Dict.keys():
            self.msg.delete(0.0, END)
            self.msg.insert(0.0, "Invalid account number!")
            self.acc.delete(0.0, END)
            num = int(self.acc.get())
        self.msg.delete(0.0, END)
        self.msg.insert(0.0, "Balance: " + str(Dict[num][1]))


class Deposit(Frame):
    def __init__(self, master):
        Frame.__init__(self, master)
        self.grid()
        self.create_widgets()

    def create_widgets(self):
        self.label1 = Label(self)
        self.label1["text"] = "   "
        self.label1.grid(row=0, column=0, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter  account number"
        self.label.grid(row=0, column=1, sticky=S)
        self.acc = Entry(self,width="50",borderwidth=15)
        self.acc.grid(row=1, column=1, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter the amount to be deposited."
        self.label.grid(row=3, column=1, sticky=S)

        self.amt = Entry(self,width="50",borderwidth=15)
        self.amt.grid(row=4, column=1, sticky=S)
        self.button = Button(self,width=25,fg="black",bg="pink",font=("Calibri",15))
        self.button["text"] = "SUBMIT"
        self.button["command"] = self.collect_data
        self.button.grid(row=6, column=1, sticky=S)
        self.msg = Text(self, width=30, height=5, wrap=WORD,borderwidth=16)
        self.msg.insert(0.0, "Final amount will be displayed here.")
        self.msg.grid(row=5, column=1, sticky=S)

    def collect_data(self):
        global Dict
        num = int(self.acc.get())
        if not num in Dict.keys():
            self.msg.delete(0.0, END)
            self.msg.insert(0.0, "Invalid account number!")
            self.acc.delete(0.0, END)
            num = int(self.acc.get())
        add = float(self.amt.get())
        Dict[num][1] = Dict[num][1] + add
        self.msg.delete(0.0, END)
        self.msg.insert(0.0, "Balance: " + str(Dict[num][1]))


class Withdraw(Frame):
    def __init__(self, master):
        Frame.__init__(self, master)
        self.grid()
        self.create_widgets()

    def create_widgets(self):
        self.label1 = Label(self)
        self.label1["text"] = "   "
        self.label1.grid(row=0, column=0, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter  account number"
        self.label.grid(row=0, column=1, sticky=S)
        self.acc = Entry(self,width="50",borderwidth=15)
        self.acc.grid(row=1, column=1, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter the amount to be withdrawn."
        self.label.grid(row=3, column=1, sticky=S)

        self.amt = Entry(self,width="50",borderwidth=15)
        self.amt.grid(row=4, column=1, sticky=S)
        self.button = Button(self,width=25,fg="black",bg="pink",font=("Calibri",15))
        self.button["text"] = "SUBMIT"
        self.button["command"] = self.collect_data
        self.button.grid(row=6, column=1, sticky=S)
        self.msg = Text(self, width=30, height=5, wrap=WORD,borderwidth=16)
        self.msg.insert(0.0, "Final amount will be displayed here.")
        self.msg.grid(row=5, column=1, sticky=S)

    def collect_data(self):
        global Dict
        num = int(self.acc.get())
        if not num in Dict.keys():
            self.msg.delete(0.0, END)
            self.msg.insert(0.0, "Invalid account number!")
            self.acc.delete(0.0, END)
            num = int(self.acc.get())
        add = float(self.amt.get())
        if add <= Dict[num][1]:
            Dict[num][1] = Dict[num][1] - add
            self.msg.delete(0.0, END)
            self.msg.insert(0.0, "Balance: " + str(Dict[num][1]))
        else:
            self.msg.delete(0.0, END)
            self.msg.insert(0.0, "Amount insufficient!")


class Search(Frame):
    def __init__(self, master):
        Frame.__init__(self, master)
        self.grid()
        self.create_widgets()

    def create_widgets(self):
        self.label1 = Label(self)
        self.label1["text"] = "   "
        self.label1.grid(row=0, column=0, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter name(full or partial)"
        self.label.grid(row=0, column=1, sticky=S)
        self.name = Entry(self,width="50",borderwidth=15)
        self.name.grid(row=1, column=1, sticky=S)

        self.button = Button(self,width=25,fg="black",bg="pink",font=("Calibri",15))
        self.button["text"] = "SUBMIT"
        self.button["command"] = self.collect_data
        self.button.grid(row=6, column=1, sticky=S)
        self.msg = Text(self, width=30, height=5, wrap=WORD,borderwidth=16)
        self.msg.insert(0.0, " ")
        self.msg.grid(row=5, column=1, sticky=S)

    def collect_data(self):
        global Dict
        name = self.name.get()
        for num in Dict.keys():
            if Dict[num][0].find(name) != -1:
                self.msg.delete(0.0, END)
                self.msg.insert(0.0, "Name: " + Dict[num][0] + "\nAccount Number: " + str(num))
                return
        self.msg.delete(0.0, END)
        self.msg.insert(0.0, "Account not found.")


class Close(Frame):
    def __init__(self, master):
        Frame.__init__(self, master)
        self.grid()
        self.create_widgets()

    def create_widgets(self):
        self.label1 = Label(self)
        self.label1["text"] = "   "
        self.label1.grid(row=0, column=0, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter  account number"
        self.label.grid(row=0, column=1, sticky=S)
        self.acc = Entry(self,width="50",borderwidth=15)
        self.acc.grid(row=1, column=1, sticky=S)
        self.button = Button(self,width=25,fg="black",bg="pink",font=("Calibri",15))
        self.button["text"] = "SUBMIT"
        self.button["command"] = self.collect_data
        self.button.grid(row=2, column=1, sticky=S)
        self.msg = Text(self, width=30, height=5, wrap=WORD,borderwidth=16)
        self.msg.insert(0.0, "  ")
        self.msg.grid(row=3, column=1, sticky=S)

    def collect_data(self):
        global Dict
        global accno
        num = int(self.acc.get())
        if not num in Dict.keys():
            self.msg.delete(0.0, END)
            self.msg.insert(0.0, "Invalid account number!")
            self.acc.delete(0.0, END)
            num = int(self.acc.get())
        self.msg.insert(0.0, "amount to be refunded: " + str(Dict[num][1]))
        del Dict[num]
        print(Dict)
        accno -= 1


class Import(Frame):
    def __init__(self, master):
        Frame.__init__(self, master)
        self.grid()
        self.create_widgets()

    def create_widgets(self):
        self.label1 = Label(self)
        self.label1["text"] = "   "
        self.label1.grid(row=0, column=0, sticky=S)
        self.label = Label(self,fg="red",font=("Comic Sans MS",20),padx=70)
        self.label["text"] = "Enter file."
        self.label.grid(row=0, column=1, sticky=S)
        self.file = Entry(self,width="50",borderwidth=15)
        self.file.grid(row=1, column=1, sticky=S)
        self.button = Button(self,width=25,fg="black",bg="pink",font=("Calibri",15))
        self.button["text"] = "SUBMIT"
        self.button["command"] = self.collect_data
        self.button.grid(row=2, column=1, sticky=S)
        self.msg = Text(self, width=30, height=5, wrap=WORD,borderwidth=16)
        self.msg.insert(0.0, "  ")
        self.msg.grid(row=3, column=1, sticky=S)

    def collect_data(self):
        global Dict
        global accno
        f = self.file.get()
        fi = open(f, 'r')
        up = open("unprocesed_accounts.txt", 'w')
        n = 0
        for line in fi:
            List = line.split(',');
            if int(List[1]) in Dict.keys():
                n = int(List[1])
            else:
                n = accno
                accno += 1
            Dict[n] = [List[0], float(List[2].rstrip('\n'))]
            if List[1] != '-1':
                l = str(n) + '\t' + Dict[n][0] + '\t' + str(Dict[n][1]) + '\n'
                up.write(l)
                del Dict[n]
        self.msg.insert(0.0, "operation successful!")
        fi.close()
        up.close()


def win1():
    root = Tk()
    root.title("Create Account")
    root.geometry("600x500")
    Create(root)
    root.mainloop()


def win2():
    root = Tk()
    root.title("Balance Enquiry")
    root.geometry("600x500")
    root = Balance(root)
    root.mainloop()


def win3():
    root = Tk()
    root.title("Deposit Amount")
    root.geometry("600x500")
    root = Deposit(root)
    root.mainloop()


def win4():
    root = Tk()
    root.title("Withdraw  Amount")
    root.geometry("600x500")
    root = Withdraw(root)
    root.mainloop()


def win5():
    root = Tk()
    root.title("Search")
    root.geometry("500x500")
    root = Search(root)
    root.mainloop()


def win6():
    root = Tk()
    root.title("Close Account")
    root.geometry("500x500")
    root = Close(root)
    root.mainloop()


def win7():
    root = Tk()
    root.title("Import Accounts")
    root.geometry("500x500")
    root = Import(root)
    root.mainloop()


def quit_fun():
    global Dict
    fi = open("file_det.txt", 'w')
    # Dict=sorted(Dict)
    print(Dict)
    for num in Dict:
        List = []
        List.rootend(str(num))
        List.rootend(Dict[num][0])
        List.rootend(str(Dict[num][1]))
        string = '\t'.join(List) + '\n'
        print(string)
        fi.write(string)
    print('successful')


root = Tk()
root.title("EMPLOYEE SALARY  MANAGEMENT")
root.geometry("750x400")
#root = Frame(root,borderwidth=20,bg="blue",height=500,width=500,relief='raised',)
#root.pack()



label = Label(root,bg="black",fg="red",anchor=W,justify=LEFT,font=("Arial Black", 25))
label["text"] = "Welcome to TinguMingu Company"
label.grid(row=0, column=2, columnspan=5)


button1 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button1["text"] = "Create Account"
button1["command"] = win1
button1.grid(row=4, column=3,padx=20)

button2 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button2["text"] = "Balance Enquiry"
button2["command"] = win2
button2.grid(row=4, column=6, sticky=S)

button3 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button3["text"] = "Deposit"
button3["command"] = win3
button3.grid(row=5, column=3, sticky=S)

button4 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button4["text"] = "Withdraw"
button4["command"] = win4
button4.grid(row=5, column=6, sticky=S)

button5 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button5["text"] = "Search by Name"
button5["command"] = win5
button5.grid(row=6, column=3, sticky=S)

button6 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button6["text"] = "Delete Account"
button6["command"] = win6
button6.grid(row=6, column=6, sticky=S)

button7 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button7["text"] = "Import Accounts"
button7["command"] = win7
button7.grid(row=7, column=3, sticky=S)

button8 = Button(root,width=25,fg="black",bg="pink",font=("Segoe Print",15))
button8["text"] = "Save"
button8["command"] = quit_fun
button8.grid(row=7, column=6, sticky=S)

root.mainloop()




