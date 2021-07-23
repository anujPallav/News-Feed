# News-Feed
Get you news on you homepage.

from tkinter import*
import time
import requests
import json

root = Tk()
root.title('News-Feed')
root.attributes('-alpha',0.9)
root.config(bg='#083e55')

icon = PhotoImage(file='icon.png')
root.iconphoto(False, icon)

country_search = Entry(root,font='bold 9',justify=CENTER,)
country_search.place(x=200,y=10, width=100,height=25)

def time_status():
    t_dict = (time.localtime())
    t_status ='Time: '+ str(t_dict[3])+':'+str(t_dict[4])+':'+str(t_dict[5])+' '+'M:'+str(t_dict[1])+'-D:'+str(t_dict[2])+'-Y:'+str(t_dict[0])
    time_label = Label(root,width=24,text=t_status,font='monospace 10 bold',justify=LEFT,anchor=W)
    time_label.place(x=0,y=10)
    root.after(1000,time_status)
time_status()



def _country_search():
    country_name = country_search.get()
    print(country_name)
    return country_name

index = -1
def news_feed():
    try:

        global index
        index = index + 1
        
        if index >= 19:
            index = 0

        url = 'https://newsapi.org/v2/top-headlines?country='+_country_search()+'&apiKey=bc0834452c79409c91b19bb42a7e429e'
        req = requests.get(url)
        content = json.loads(req.content)
        
        
        title_label = Label(root,width=400, text='Title: '+content['articles'][index]['title'], font='oblique  10 bold',background='#2E4053',
        fg='white',height=5,padx=0,justify=LEFT,wraplength=500,anchor=W) 
        title_label.place(x=0,y=50)

        Discription =Label(root,width=400, text='Description: '+ content['articles'][index]['description'], font='oblique  10 bold',
        background='#2E4053',fg='white',height=10,padx=0,justify=LEFT,wraplength=500,anchor=W) 
        Discription.place(x=0,y=135)

        next_button = Button(root, text='Next-Headline',command=news_feed)
        next_button.place(x=200,y=320)

        status_bar = Label(root,width=100,text=' This is page: '+str(index)+' headline and description.',
        background='white',fg='black',anchor=W).place(x=0,y=370)
    except:
        print('Error')


def show_headlines():
    news_feed()

country_search_button = Button(root,text='Search Country', justify=CENTER
,command=lambda:[ _country_search(), show_headlines()])
country_search_button.place(x=305,y=10)
                
root.minsize(550,390)
root.maxsize(550,390)
root.mainloop()
