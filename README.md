Have solved all the 3 questions along with the proof of namedtuples being faster than dict (even for a single run, without averaging) but could not understand how to write the test cases part, so no github actions. :(

Uploads: Colab notebook .ipynb files separately for each question1, 2 and 3: with logs, comments and docstrings.

# Execution time differece: 
    Namedtuple: 14.056909721000011 sec
    Dict: 15.485492480000175 sec
  
#Explanation of each of the 3 solutions:

# Solution 1. Use the Faker library to get 10000 random profiles. Using namedtuple, calculate the largest blood type, mean-current_location, oldest_person_age, and average age

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
# Solution 2:  
  
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
