Have solved all the 3 questions along with the proof of namedtuples being faster than dict (even for a single run, without averaging) but could not understand how to write the test cases part, so no github actions. :(

Uploads: Colab notebook .ipynb files separately for each question1, 2 and 3: with logs, comments and docstrings.

Execution time differece: 
  Namedtuple: 14.056909721000011 sec
  Dict: 15.485492480000175 sec
  
Explanation of each of the 3 solutions:

1: Use the Faker library to get 10000 random profiles. Using namedtuple, calculate the largest blood type, mean-current_location, oldest_person_age, and average age

  Solution steps:
        step1 - create a namedtuple field
        step2 - write algo to take all values from dict to namedtuple
        step3 - merge all namedtuples into 1 tuple
        step4 - start calculation : alculate the largest blood type, mean-current_location, oldest_person_age, and average age

Block 1:
                      from time import perf_counter
                      start= perf_counter()
                      Faker.seed(0)
                      fake = Faker()
                      fp={}                            # declared a dic
                      profile_count=10_000
                      for i in range(profile_count):
                        fp[i]=fake.profile()           # output dict
                      print(type(fp))
