#!/usr/bin/python
import requests
import json
import csv
import os
import sys
app_id='c696b774'
app_key='0b75d11a71ea39f84546c8e4ad612550'
language='en'
word_id=raw_input("enter the word you want to search")
if os.path.isfile('/home/gupta/Desktop/cod/CompletionReportOutput.csv'):
	f=open('/home/gupta/Desktop/cod/CompletionReportOutput.csv','r')
	for line in f.readlines():
		line = line.strip()
		columns = line.split()
		name = columns[0]
		if name==word_id:
			print(line)
			sys.exit()
	f.close()
	url='https://od-api.oxforddictionaries.com:443/api/v1/entries/'+language+'/'+word_id.lower()+'/definitions'
	r=requests.get(url, headers={'app_id':app_id,'app_key':app_key})
	te=r.json()
	r=te["results"][0]["lexicalEntries"][0]["entries"][0]["senses"][0]["definitions"]
	with open('/home/gupta/Desktop/cod/CompletionReportOutput.csv','a') as fp:
		a=csv.writer(fp)
		te=json.dumps(r)
		data=[[word_id+"  ",te]]
		a.writerows(data)
	print(r)
else:
	url='https://od-api.oxforddictionaries.com:443/api/v1/entries/'+language+'/'+word_id.lower()+'/definitions'
	r=requests.get(url, headers={'app_id':app_id,'app_key':app_key})
	te=r.json()
	r=te["results"][0]["lexicalEntries"][0]["entries"][0]["senses"][0]["definitions"]
	with open('/home/gupta/Desktop/cod/CompletionReportOutput.csv','w') as fp:
		a=csv.writer(fp)
		te=json.dumps(r)
		data=[['word_id',' definitions'],[word_id+"  ",te]]
		a.writerows(data)
	print(r)
