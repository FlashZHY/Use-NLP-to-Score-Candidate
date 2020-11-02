# Use-NLP-to-Score-Candidate
### 1. Pick a method:
To sore the candidate using NLP, the common way is to create a keyword list and use the context of resume to match them.

There are two popular way to create the keyword list:
1. Use the related corpus (here, it is the corpus of data science); create keyword list using model of `Word2Vec`; use `model.wv.most_similar` function to find the desired keyword list of each field. eg. use 
`model.wv.most_similar ("scripting languages")` to find the keyword list of scripting languages; use that list to match resume.


2. Create the key list using the given dataset. Here, go through the 74 resumes and extract keyword list for each field and use that to match resume.

The 1st method saves a lot of time searching the keyword in dataset. But it has 2 main disadvantages:
- `Require large and accurate dataset` -When the appropriate dataset can’t be found, the corresponding keyword list would be less relevant.
- `Require much time to train the model` -When the dataset is large, it would take a lot of time to process it.

After getting an irrelevant keyword list using method 1, I chose to create list by myself.

### 2. Create the keyword list:
I combined the 6 requirements in the "Qualifications" in to three categories and pointed out the area where you can find these key requirements in the dic:


1. **Education**



- A: `Major` - "education"


2. **Working Experience**


- B: `5-10 years of experience in data science` - most in "strength"

- C: `Experience working with international partners` - "strength" & "job description"



3. **Hard skills**


- D: `Date science model`  - "strength" & "job description"

- E: `Large data set & analytical tool`  - "strength" & "job description"

- F: `Scripting languages`  - "strength" & "job description"

I **separate** the dataset to 3 parts: `"education"`, `"strength"` and `"strength & job description"` for different requirements to get a **more precise** result.

For the requirements using the same text (eg. CDEF), I matched them together.

Some notes:
- For all the requirements, I recorded the keyword by going through **all** 74 candidates' resumes to make my keywords list comprehensive.


- For hard skills, I overlooked some of them if they only appear once in the dataset. 


- Though I tried very hard to figure out the hard skills and applied them to the right categories, I may still miss some of them when I am not able to recognize them as hard skills


- B is a little bit **different** from the others because I have to filter the length of working experience first, which would be discussed later.

### 3. Build up a scoring model:
When scoring the candidates in each category, the criterion differs based on the features of the requirements.

- **Education** and **Working Experience** are `"Yes or No"` questions, which means you will get limited credit if your major is on the list or you have the related working experience. You will not get extra points if you have 2 related majors (eg. one in bachelor, one in master) or you have 2 global working experiences.


- **Hard skills** are `"The More the Better"` questions, which means if you have more skills, you can get more points for them without upper bound.

As a result, I assigned `10 points` for A, B & C and `3 points` for any matched (In one resume, multiple matches of one same skill would be only considered "one match". Only different skills can make the score higher) hard skill in D, E & F.

Then I calculated the sum of the points in A, B, C, D, E, F and got the final score and rank.
