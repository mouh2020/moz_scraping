from tkinter import Tk     # from tkinter import Tk for Python 3.x
from tkinter.filedialog import askopenfilename
import requests,json
import time


basic_auth = ('mozID', "api_token")
api_endpoint = "https://lsapi.seomoz.com/v2/url_metrics"
data = []


def import_urls(filename) :
    return [url.strip() for url in open(filename,'r').readlines()]





def do_extraction (urls) :
    test= {"targets": urls}

    try :
        response = requests.post(api_endpoint,auth=basic_auth,data=json.dumps(test),verify=True)
    except Exception as e :
        print(e)
    response = (response.json())['results']
    for i in range(len(response)) :
        data.append({'url':urls[i],
                     'spam_score': 0 if response[i]['spam_score'] == -1 else response[i]['spam_score'],
                     'domain_authority' : response[i]['domain_authority']})


def extraction (urls) :
    ################# if urls less or equal to 50  >> do one request
    if len(urls) <= 50 :
        do_extraction(urls=urls)
        
    ################## else do it step by step (step size : 50) respecting api rate limits
    else :
        to_be_requested_urls = []
        for i in range(len(urls)):
            to_be_requested_urls.append(urls[i])
            if len(to_be_requested_urls) == 50 :
                ## make request of 50 urls
                do_extraction(urls = to_be_requested_urls )
                to_be_requested_urls = []
                ## sleep 10 s respecting api rate limits
                print("\n - Waiting for 10 s ")
                time.sleep(10)
        if len(to_be_requested_urls) != 0 :
            do_extraction(urls = to_be_requested_urls )
                
            
            
            
            
        

def build_report (choice,data) :
    if choice == 1 : ### Authority order
        data = sorted(data, key=lambda d: d['domain_authority'],reverse=True) 
        

    else : #### Spam order
        data = sorted(data, key=lambda d: d['spam_score'],reverse=False) 
    data_to_save = ''
    for row in data :
        data_to_save += f'\n ------------------ \n{row["url"]}\nسبام سكور {row["spam_score"]}\nاثورتي {row["domain_authority"]}'
        
    with open('exported_data.txt','w',encoding="utf-8") as file :
        file.write(data_to_save)
    print(' - Finished ')



if __name__ == "__main__"  :
    Tk().withdraw()
    filename = askopenfilename()
    urls =  import_urls(filename)
    print(f'\n - {len(urls)} domains imported')
    print('\n 1 ) Authority\n 2 ) Spam\n')
    choice = int(input(' - Enter your choice number : '))
    extraction (urls) ####### prepare urls and extract
    build_report(choice,data)

        
    
        
