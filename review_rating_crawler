#import libraries
import urllib    
from bs4 import BeautifulSoup
import csv

#######################


#create list with seed urls (i.e., the URLS from the SERP)
#you can manually add these serach results pages
seeder1 = 'https://www.imdb.com/search/keyword/?keywords=vaccine&ref_=kw_ref_rt_vt&sort=moviemeter,asc&mode=detail&page=1&num_votes=100%2C'
seeder2 = 'https://www.imdb.com/search/keyword/?keywords=vaccine&ref_=kw_nxt&mode=detail&page=2&num_votes=100%2C&sort=moviemeter,asc'
seeder3 = 'https://www.imdb.com/search/keyword/?keywords=vaccine&ref_=kw_nxt&mode=detail&page=3&num_votes=100%2C&sort=moviemeter,asc'
seeder_list_SERP = []
seeder_list_SERP.append(seeder1)
seeder_list_SERP.append(seeder2)
seeder_list_SERP.append(seeder3)

#print(seeder_list_SERP,"\n") #print the list

#count the entries (as a test)
seeder_list_SERP_count = 0
for i in seeder_list_SERP:
    seeder_list_SERP_count += 1
print("seeder_list_SERP_count is ", seeder_list_SERP_count,"\n")
#print(seeder_list_SERP)



#######################



#grab all the title URls from IMDB SERP

title_href_list = [] #create the list placeholder for the Title urls, 
#.. obtained from the seed URLS

for seed in seeder_list_SERP:
    url = seed
    resp = urllib.request.urlopen(url)
    soup = BeautifulSoup(resp, from_encoding=resp.info().get_param('charset'))  # Get server encoding 
    external_links = set()
    internal_links = set()
    for line in soup.find_all('a'): #grab the a's
        link = line.get('href') #grab the hrefs
        if not link:
            continue
        if link.startswith('http'):
            external_links.add(link)
        else:
            internal_links.add(link)
            
    full_internal_links = {
        urllib.parse.urljoin(url, internal_link) 
        for internal_link in internal_links
    }

#write the href list for the TITLES

    for link in external_links.union(full_internal_links):
        if '/title/tt' in link:
            if 'plotsummary' not in link:
                if 'vote' not in link:
                       title_href_list.append(link)

print(title_href_list)
a = 0
for link in title_href_list:
    a+=1
    
print("\nTitle URLs count is ", a,"\n") 
#This picks the Title of tv shows 
#and their associated Episode page and potential dupes too 



#######################


#remove dupes by converting to a set 
set_test = set(title_href_list)
#test start
a = 0
for link in set_test:
    a+=1
#print("\nTitle URLs count is ", a,"\n") 





#######################





#now swap back the deduped set to a list
title_href_list = list(set_test)
#test start
a = 0
for link in title_href_list:
    a+=1
print("\nTitle URLs count is ", a,"\n")
#test end




#######################



title_review_href_list = [] #create blank list for individual title 'review' pages to be added to 

#need to loop through the Titles list to pull all urls with 'review' from..
#..each title URL - i.e., i need the Review URL for each title 
for title_review_url in title_href_list: 
        url = title_review_url #assign new TITLE url for each iteration
        resp = urllib.request.urlopen(url)
        soup = BeautifulSoup(resp, from_encoding=resp.info().get_param('charset')) # server encoding   
        external_links = set()
        internal_links = set()
        for line in soup.find_all('a'):
            link = line.get('href')
            if not link:
                continue
            if link.startswith('http'):
                external_links.add(link)
            else:
                internal_links.add(link)

        full_internal_links = {
            urllib.parse.urljoin(url, internal_link) 
            for internal_link in internal_links
        }

        for link in external_links.union(full_internal_links):
            if 'review' in link:
                title_review_href_list.append(link)
                
print(title_review_href_list) #print the list of URLS with Review in the title


#######################

#count the list entries - ..
#.. expect to be higher than count of Title pages
#test start#
a = 0
for link in title_review_href_list:
    a+=1
    
print("\nMy LIST of Review-Titles (e.g., tt0796366/reviews) (not-deduped) URLs count is ", a,"\n") 
#test end


#######################

#dedupe the list by converting to a set
title_review_href_SET_deduped = set(title_review_href_list)
#test start#
a = 0
for link in title_review_href_SET_deduped:
    a+=1
    
print("\nMy SET of Review-Titles (e.g., tt0796366/reviews) (deduped) URLs count is ", a,"\n") 
#test end




#######################


#Dedupe and convert back to a list
#remove the critic..
#..and external reviews (might want these later so create new list)

title_review_href_LIST_deduped_cleaned = [] #create the list for deduped and cleaned reviews

for link in title_review_href_SET_deduped:
    if 'criticreviews' not in link:
        if 'externalreviews' not in link:
            title_review_href_LIST_deduped_cleaned.append(link) #should be equal to num of titles

#test Start
a = 0
for link in title_review_href_LIST_deduped_cleaned:
    a+=1
print(title_review_href_LIST_deduped_cleaned)   
print("\nReview (cleaned) URLs count is ", a,"\n") 

#test end



#######################





#next - using "*//title/*/reviews", grab all individual review urls.

individual_reviews_perma_list = [] #create blank list for..
#..'review' hrefs to be added to 

for individual_review_url in title_review_href_LIST_deduped_cleaned: 
        url = individual_review_url #assign the relevant review url for each iteration
        resp = urllib.request.urlopen(url)
        soup = BeautifulSoup(resp, from_encoding=resp.info().get_param('charset')) # server encoding   
        external_links = set()
        internal_links = set()
        for line in soup.find_all('a'):
            link = line.get('href')
            if not link:
                continue
            if link.startswith('http'):
                external_links.add(link)
            else:
                internal_links.add(link)

        full_internal_links = {
            urllib.parse.urljoin(url, internal_link) 
            for internal_link in internal_links
        }

        for link in external_links.union(full_internal_links):
            if 'review/rw' in link: #common feature in all review permalinks
                individual_reviews_perma_list.append(link) #this new list contains the individual permalink to each individual review





#######################


#start test for dupes
set_test_final = set(individual_reviews_perma_list)
a = 0
for link in set_test_final:
    a+=1
    
print("\n Individual Review URLS count is ", a,"\n")
#end test




#######################

