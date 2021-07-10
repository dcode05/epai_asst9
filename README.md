Have solved all the 3 questions along with the proof of namedtuples being faster than dict (even for a single run, without averaging) but could not understand how to write the test cases part, so no github actions. :(

Uploads: Colab notebook .ipynb files separately for each question1, 2 and 3: with logs, comments and docstrings.

# Execution time differece: 
    Namedtuple: 14.056909721000011 sec
    Dict: 15.485492480000175 sec
  
#Explanation of each of the 3 solutions:

# Solution 1.  Using namedtuple, calculate the largest blood type, mean-current_location, oldest_person_age, and average age

  # Solution steps:
        step1 - create a namedtuple field
        step2 - write algo to take all values from dict to namedtuple
        step3 - merge all namedtuples into 1 tuple
        step4 - start calculation : alculate the largest blood type, mean-current_location, oldest_person_age, and average age

# Block 1: storing people profiles in a dict
        from time import perf_counter
        start= perf_counter()
        Faker.seed(0)
        fake = Faker()
        fp={}                            # declared a dic
        profile_count=10_000
        for i in range(profile_count):
            fp[i]=fake.profile()           # output dict
        print(type(fp))

# step 1 : namedtuple is defined with keys from 'fp' dict above
        fake_profile = namedtuple('fake_profile', sorted(fp[0].keys()))
        print(fake_profile._fields)
        print(len(fake_profile._fields))

# step 2 written algo to take all values from dict to namedtuple
 
        fake_profile_tuple={}    #-------> defined/initialized as dict

        for i in range (profile_count):
          fake_profile_tuple[i] = fake_profile(**fp[i])
        
        print(fake_profile_tuple[0])
    
        
# # step3 -  merged all namedtuples and start calculation: largest blood type, mean-current_location, oldest_person_age, and average age
            
        tuple_merged= ()

        for i in range(profile_count):
          tuple_merged= tuple_merged+ (fake_profile_tuple[i],)
            
        
# step4 -  start calculation : calculate the largest blood type, mean-current_location, oldest_person_age, and average age

        import datetime
        # birthdates = tuple([t[1] for t in tuple_merged])    # give tuple of bithdates of all profiles 
        birthdates = [t[1] for t in tuple_merged]               # give list of bithdates of all profiles ----> faster than tuple
        oldest=min( birthdates )
        age_oldest= datetime.date.today() - oldest          # type: datetime.timedelta


        index_oldest=birthdates.index(oldest)
        oldest_person=tuple_merged[index_oldest][7]

        print('bithdates',birthdates)
        print('oldest', oldest)
        print('age_oldest',age_oldest)       
        
#calculation2: average age calculation

        age= [(datetime.date.today()-t[1]) for t in tuple_merged] 
        age_total= age[0]
        for i in range(profile_count-1):
          age_total+= age[i+1]
        age_average= age_total/ len(age)       # type:   datetime.timedelta
        print('age_average',age_average)        
        
#calculation3: largest blood type

        bloodtype = [t[2] for t in tuple_merged] 
        def most_frequent(bloodtype):
            return max(set(bloodtype), key = bloodtype.count)

        print(most_frequent(bloodtype))       
        
#calculation4: mean-current_location

        current_location = [t[4] for t in tuple_merged] 
        mean_location= sum(t[0] for t in current_location)/profile_count, sum(t[1] for t in current_location)/profile_count
        print('mean_location is ', mean_location)        

# -----------------------------------------------------------------------------------------------------------------------------
# Solution 2:  Using dict, calculate the largest blood type, mean-current_location, oldest_person_age, and average age
  
  # STEP1: IMPORTING & STORING PROFILTES IN A DICT
        from time import perf_counter
        start= perf_counter()
        Faker.seed(0)
        fake = Faker()
        fp={}                            # declared a dic
        profile_count=10_000
        for i in range(profile_count):
          fp[i]=fake.profile()           # output dict- stores all profiles 
        
  # STEP2: creating a new list of bloodtypes
  
        blood_group_list=list()                                 # list decleration              
        for i in range(profile_count):                        
          blood_group_list.append(fp[i]["blood_group"])          # all the blood groups stored here

        # print(blood_group_list) 


  # STEP3: CALCULATION OF LARGEST BLOOD TYPE
        
        def most_frequent(List):
            return max(set(List), key = List.count)

        most_frequent(blood_group_list)                 # gives the boodtype which occurs most frequently

# STEP: 4 : creating a new list of mean location and using sum() and list comprehension to calculate the mean location of profiles

        current_location_list=list()                               # list decleration 
        for i in range(profile_count):                        
            current_location_list.append(fp[i]["current_location"])          # all the mean location  stored here

        print(current_location_list)
        mean_location= sum(t[0] for t in current_location_list)/profile_count, sum(t[1] for t in current_location_list)/profile_count
        mean_location


# STEP: 5: oldest_person_age

        birthdate_list=list()                              # list decleration 
        for i in range(profile_count):                        
          birthdate_list.append(fp[i]["birthdate"])          # all the bithdates stored here

        print(birthdate_list)
        import datetime

        oldest=min( birthdate_list )
        age_oldest= datetime.date.today() - oldest 
        index_oldest=birthdate_list.index(oldest)

        print(oldest)
        print(age_oldest)
        print(index_oldest)

        
# STEP: 6: average age calc
        age=  [ datetime.date.today()-t for t in birthdate_list ]         # calculates age of each member in the profile
        age_total= age[0]
        for i in range(profile_count-1):
          age_total+= age[i+1]
        age_average= age_total/ len(age)  
        print(age_average)
        
# -----------------------------------------------------------------------------------------------------------------------------
Solution 3: Create fake data (you can use Faker for company names)for imaginary stock exchange for top 100 companies (name,symbol, open, high, close). Assign a random weight to all the companies. Calculate and show what value the stock market started at, what was the highest value during the day, and where did it end. Make sure your open, high, close are not totally random. You can only use namedtuple.
       
# steps:

    download and store company names
    infer company symbols from company names
    make a list of weight, open, high, close
    unpack symbol list and data list into the stock namedtuple
    perform required calculations
        starting value of stock market
        highest value of stock market
        ending value of stock market

# step1: store company names in a list
        Faker.seed(0)
        fake = Faker()

        comp_name=list()                            # declared a dic
        company_count=100

        for _ in range(company_count):
          comp_name.append( fake.company())
          # print(comp_name)

        print(type(comp_name))
        print(comp_name)

# step 2: store the stock symbols in another list

        comp_name_sym= list()
      
        comp_name_sym= [[t[0:3]] for t in comp_name]
        print(comp_name_sym)
        print(type(comp_name_sym[0]))
        print(len((comp_name_sym[0])))
# step 3: make a list of weight, open, high, close   az=stock('symbol','weight', 'open', 'high', 'close')

        data_list= list()    #each item= weight, open, high, close

        import random as ran
        from random import random

        open = [ran.randint(50,500) for i in range(company_count)]
        high= [t+50 for t in open]
        close=[t+20 for t in open]

        weight = [random() for i in range(company_count)]
        s = sum(weight)
        weight = [ i/s for i in weight ]


        for i in range(company_count):
          data_list.append([weight[i],open[i],high[i],close[i]])


        print('open',open, 'weight',weight)
        print(type(data_list))
        print(len(data_list))
        print(len(data_list[0]))
        print((data_list))

# step4: join symbol list and data list into the stock namedtuple

        stock_list=list()                        # stock_list=stock('symbol','weight', 'open', 'high', 'close')

        for i in range(company_count):
          stock_list.append([stock(*comp_name_sym[i],*data_list[i])])

        print(stock_list)
        
# actual calculation: 
    # 1. open price of stock market
    # 2. high price of stock market
    # 3. close price of stock market

        stockmarket_open= sum([(t[0][1]*t[0][2]) for t in stock_list])
        stockmarket_high= sum([(t[0][1]*t[0][3]) for t in stock_list])
        stockmarket_close= sum([(t[0][1]*t[0][4]) for t in stock_list])

        print('stockmarket_open',stockmarket_open)
        print('stockmarket_high',stockmarket_high)
        print('stockmarket_close',stockmarket_close)        
