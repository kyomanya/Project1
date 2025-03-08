from tkinter import *
from tkinter.ttk import Combobox
from tkinter import messagebox

f = open('слова.txt', 'r+')
d = f.read().split()
ap = {} #This vocabulary will contain english words in keys and russian words in values
for k in d:
    m, n = k.split('-')
    ap[m] = n

pa = {} #This vocabulary will contain russian words in keys and english words in values
for i in ap.keys():
    pa[ap[i]] = i


def tl(): #This function is for translating
    word = vvod.get()
    com = comb.get()
    if word in ap.keys() or word in pa.keys():
        #This 'if' is for recognising whether the word is written in english or russian
        if com == 'рус-анг':
            vyvod.configure(text=pa[word])
        elif com == 'анг-рус':
            vyvod.configure(text=ap[word])
    elif word == '':
        messagebox.showinfo('Error', 'Введите слово')
    else:
        ask = messagebox.askyesno('Error', 'Такого слова нет в словаре. Хотите добавить слово в словарь?')
        #This window shows up if the word is not in the vocabulary and asks
        # if the user wants to add this word

        if ask == True:
            vyvod.configure(text='Введите перевод слова')
            addbtn.grid(column=0, row=31)
            addent.grid(column=0, row=30)


def adding(): #This function is for adding new words in the translator
    p1 = vvod.get()
    p2 = addent.get()
    if comb.get() == 'рус-анг':
        print(p2 + '-' + p1, file=f)
    else:
        print(p1 + '-' + p2, file=f)
    f.close()


window = Tk()
window.geometry('1200x1600')
window.title('Переводчик')

lbl = Label(window, text='Введите слово', font=('Arial', 30))
lbl.grid(column=0, row=0)

vvod = Entry(window, width=50, font=('Arial', 20))
vvod.grid(column=0, row=10)
vvod.focus()

comb = Combobox(window)
comb['values'] = ('рус-анг', 'анг-рус')
comb.grid(column=1, row=10)
comb.current(1)

btn = Button(window, command=tl, width=10, text='перевод', bg='pink')
btn.grid(column=3, row=10)

addbtn = Button(window, text='Добавить слово', bg='pink', command=adding)
addent = Entry(window, width=50, font=('Arial', 20))
