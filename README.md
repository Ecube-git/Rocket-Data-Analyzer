# Rocket-Data-Analyzer


import tkinter
import numpy as np
import pandas as pd
import matplotlib
from matplotlib import pyplot as plt
from matplotlib import style
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from tkinter import *
from tkinter import filedialog
import PIL
from PIL import ImageTk,Image

root = Tk()

root.geometry("1366x768")
#root.minsize(1366,768)
#root.maxsize(1366,768)
root.overrideredirect(True)
root.geometry("{0}x{1}+0+0".format(root.winfo_screenwidth(), root.winfo_screenheight()))
root.title("D-Labz Aerospace Flight Data Analyzer ")

mainframe = Frame(root).pack()

#header begin
header = Frame(root, borderwidth=8, relief=GROOVE,bg="#35363A")
l = Label(header, text="Welcome to D-Labz Aerospace", font="Helvetica 16 bold", fg="white", bg="#35363A" , pady=22)
l.pack()
header.pack(side=TOP, fill="x")

# side bar begin
sideframe = Frame(mainframe, borderwidth=6, relief=SUNKEN, bg="#35363A")
l = Label(sideframe, text="Statistical Data",bg="#35363A" , fg="white").pack(pady=14, padx=24)

def generate():
    global  cd, lo,hm,ftd,atd,atdi,apd,sd
    filename = filedialog.askopenfilename(initialdir="/", title="Select A File", filetypes=(("csv files", "*.csv"),("all files", "*.*")))
    ab = filename
    bb = ab.split("/")
    cc = bb[-1]
    df = pd.read_csv(cc)
    td = df.column_a  # time data
    hd = df.column_b  #height data
    t = list(td) #converting time data into list
    h = list(hd) #converting height data into list
    lo = min(df.column_a) #minimun timing i.e lift off timing 
    hm = min(df.column_b) #minimun height 
    ftd = max(df.column_a) #max time from take off to landing i.e touch down timing or total crusing time
    atd = max(df.column_b) #apogee data (height)
    atdi = h.index(atd) #getting teh index of apogee height 
    apd = t[atdi]       #getting time of apogeee
    sd = atd/apd #speed = distance/time 

    lift = Frame(sideframe, borderwidth=6, relief=SUNKEN, bg="#666A79")
    l = Label(lift, text="Lift Off Timing",bg="#666A79" , fg="white").pack()
    lift.pack(fill="x")

    liftdata = Frame(sideframe, borderwidth=6,relief=SUNKEN, bg="#666A79")
    l = Label(liftdata, text= lo ,font="helvetica 28 bold", bg="#666A79", fg="white").pack(padx=55,pady=52)
    liftdata.pack(fill="x")

    apogee = Frame(sideframe, borderwidth=6, relief=SUNKEN, bg="#666A79")
    l = Label(apogee, text="Apogee Timing", bg="#666A79", fg="white").pack()
    apogee.pack(fill="x")

    apogeedata = Frame(sideframe, borderwidth=6, relief=SUNKEN ,bg="#666A79")
    l = Label(apogeedata, text=apd,font="helvetica 28 bold" , bg="#666A79" ,fg="white").pack(padx=55,pady=52)
    apogeedata.pack(fill="x")

    land = Frame(sideframe, borderwidth=6, relief=SUNKEN , bg="#666A79")
    l = Label(land, text="Touch Down Timing", bg="#666A79",fg="white").pack()
    land.pack(fill="x")

    landdata = Frame(sideframe, borderwidth=6,relief=SUNKEN , bg="#666A79" )
    l = Label(landdata, text=ftd,font="helvetica 28 bold",bg="#666A79" , fg="white").pack(padx=55,pady=52)
    landdata.pack(fill="x")

    sideframe.pack(side=LEFT, anchor="nw",fill="y")
    # side bar end

    #top frame start

    sideframe1 = Frame(mainframe, borderwidth=6, relief=SUNKEN , bg="#35363A")
    sideframe1.pack(side=TOP, anchor="nw",fill="x")

    sideframe2 = Frame(mainframe, borderwidth=6, relief=SUNKEN,bg="#666A79")  

    speedframe = Frame(sideframe2, borderwidth=6, relief=SUNKEN, bg="#666A79")
    speed = Label(speedframe , text="Max Speed ", bg="#666A79",fg="white").pack(padx=145)
    speedframe.grid(row=0, column=0)

    speeddata = Frame(sideframe2, borderwidth=6 , relief=SUNKEN , bg="#666A79")
    l = Label(speeddata, text="speed data",font="helvetica 28 bold",bg="#666A79",fg="#666A79",padx=80).grid(row=0,column=0)
    l = Label(speeddata, text=sd,font="helvetica 28 bold",bg="#666A79",fg="white").grid(row=0,column=0)
    speeddata.grid(row=1, column=0)

    altframe = Frame(sideframe2, borderwidth=6, relief=SUNKEN , bg="#666A79")
    speed = Label(altframe , text="Max Altitude",bg="#666A79",fg="white").pack(padx=145)
    altframe.grid(row=0, column=1)

    altdata = Frame(sideframe2, borderwidth=6 , relief=SUNKEN,bg="#666A79")
    l = Label(altdata, text="altitude data",font="helvetica 28 bold",bg="#666A79",fg="#666A79",padx=80).grid(row=0, column=0)
    l = Label(altdata, text=atd,font="helvetica 28 bold",bg="#666A79",fg="white").grid(row=0,column=0)
    altdata.grid(row=1, column=1)


    flightframe = Frame(sideframe2, borderwidth=6, relief=SUNKEN , bg="#666A79")
    speed = Label(flightframe , text="Total Flight Time", bg="#666A79" , fg="white").pack(padx=151)
    flightframe.grid(row=0, column=2)

    flightdata = Frame(sideframe2, borderwidth=6 , relief=SUNKEN, bg="#666A79")
    l = Label(flightdata, text="flight data",font="helvetica 28 bold",bg="#666A79", fg="#666A79",padx=107).grid(row=0,column=0)
    l = Label(flightdata, text=ftd,font="helvetica 28 bold",bg="#666A79", fg="white").grid(row=0,column=0)
    flightdata.grid(row=1, column=2)


    sideframe2.pack(side=TOP, fill="x", expand = NO)
    # top bar end


def include_graph():

    global  c
    filename = filedialog.askopenfilename(initialdir="/", title="Select A File", filetypes=(("csv files", "*.csv"),("all files", "*.*")))
    a = filename
    b = a.split("/")
    c = b[-1]

        #graph and image section start
    idframe = Frame(mainframe, borderwidth=6, relief=SUNKEN, bg="#666A79")  

    graphframe = Frame(idframe, borderwidth=6, relief=SUNKEN , bg="#666A79")
    graph = Label(graphframe , text="Graphical Data", bg="#666A79",fg="white").pack(padx=263)
    graphframe.grid(row=0, column=0)

    """ cameraframe = Frame(idframe, borderwidth=6, relief=SUNKEN, bg="#666A79")
    camera = Label(cameraframe, text="Camera Feed", bg="#666A79",fg="white").pack(padx=252)
    cameraframe.grid(row=0, column=1) """

    # graph start
    gframe = Frame(idframe,borderwidth=6, relief=SUNKEN,bg="#666A79")

    #function to include the .csv file (excel file) starts here


    
#function to include the .csv file (excel file) starts here

    sample_data = pd.read_csv(c)
    fig = plt.Figure(figsize=(5.7,4.7),dpi=100)
    fig.add_subplot(111).plot(sample_data.column_a,sample_data.column_b)
    chart = FigureCanvasTkAgg(fig,gframe)
    chart.get_tk_widget().pack()

    gframe.grid(row=1,column=0)

    # graph end

    
    
    idframe.pack(side=LEFT)
    #graph section end

#image section start

def include_igraph1():
    """ global  c1
    filename = filedialog.askopenfilename(initialdir="/", title="Select A File", filetypes=(("csv files", "*.csv"),("all files", "*.*")))
    a1 = filename
    bc = a1.split("/")
    c1 = bc[-1] """

    idframe = Frame(mainframe, borderwidth=6, relief=SUNKEN, bg="#666A79")  
    #graphframe = Frame(idframe, borderwidth=6, relief=SUNKEN , bg="#666A79")
    #graph = Label(graphframe , text="Internal Camera", bg="#666A79",fg="white").pack(padx=240)
    #graphframe.grid(row=0, column=1)

    #function to include the .csv file (excel file) starts here
    #gframe = Frame(idframe,borderwidth=6, relief=SUNKEN,bg="#666A79")
    """    sample_data = pd.read_csv(c1)
    fig = plt.Figure(figsize=(5.7,4.7),dpi=100)
    fig.add_subplot(111).plot(sample_data.column_a,sample_data.column_b)
    chart = FigureCanvasTkAgg(fig,gframe)
    chart.get_tk_widget().pack()
    """
    
    my_img1 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img2 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img3 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img4 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img5 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img6 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img7 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img8 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img9 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img10 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img11 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img12 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img13 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img14 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))
    my_img15 = ImageTk.PhotoImage(Image.open("D:\harsh\work\startup\python program - Copy\design.png"))

    image_list = [my_img1,my_img2,my_img3,my_img4,my_img5,my_img6,my_img7,my_img8,my_img9,my_img10,my_img11,my_img12,my_img13,my_img14,my_img15]


    my_label = Label(idframe,image=my_img1)
    my_label.grid(row=0,column=0)

    def forward(image_number):
        global my_label
        global button_forward
        global button_backward

        my_label.grid_forget()
        my_label = Label(idframe,image=image_list[image_number-1])
        button_backward = Button(idframe,text="<<", command=lambda : backward(image_number-1))
        button_forward = Button(idframe,root,text=">>", command=lambda : forward(image_number+1))

        if image_number == 15:
            button_forward = Button(idframe,text=">>", command=lambda : forward(image_number+1),state=DISABLED)

        my_label.grid(row=0,column=0)
        button_backward.grid(row=1,column=0)
        button_forward.grid(row=1,column=2)

        


    def backward():
        global my_label
        global button_forward
        global button_backward

        my_label.grid_forget()
        my_label = Label(idframe,image=image_list[image_number-1])
        button_backward = Button(idframe,text="<<", command=lambda : backward(image_number-1))
        button_forward = Button(idframe,text=">>", command=lambda : forward(image_number+1))

        if image_number == 1:
            button_backward = Button(idframe,text="<<", command=lambda : backward(image_number+1),state=DISABLED)

        my_label.grid(row=0,column=0)
        button_backward.grid(row=1,column=0)
        button_forward.grid(row=1,column=2)


    button_backward = Button(idframe,text="<<", command=backward).grid(row=1,column=0)
    button_forward = Button(idframe,text=">>", command=lambda :forward(2)).grid(row=1,column=2)




    #gframe.grid(row=1,column=1)
    
    idframe.pack(side=TOP,anchor="ne",fill=BOTH)
    #image section start

#button section start
butframe = Frame(mainframe, borderwidth=6, relief=SUNKEN, bg="#35363A")  
b3 = Button(butframe,text="Generate",font="Helvetica 12 bold",bg="#35363A", fg="white",command=generate,padx=130).grid(row=0,column=0)
b1 = Button(butframe,text="Import Data",font="Helvetica 12 bold",bg="#35363A",fg="white",padx=112,command=include_graph).grid(row=0,column=1)
b2 = Button(butframe,text="Import Images",font="Helvetica 12 bold",bg="#35363A", fg="white",padx=111,command=include_igraph1).grid(row=0,column=2)
b = Button(butframe,text="Quit",font="Helvetica 12 bold",bg="#35363A",fg="white",command=exit,padx=150).grid(row=0,column=3)


butframe.pack(side=BOTTOM, fill='x')
#button section end



root.mainloop()

