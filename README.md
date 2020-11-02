# Use-NLP-to-Score-Candidate
To sore the candidate using NLP, the common way is to create a keyword list and use the context of resume to match them.

There are two popular way to create the keyword list:
1. Use the related corpus (here, it is the corpus of data science); create keyword list using model of `Word2Vec`; use `model.wv.most_similar` function to find the desired keyword list of each field. eg. use 
`model.wv.most_similar ("scripting languages")` to find the keyword list of scripting languages; use that list to match resume.


2. Create the key list using the given dataset. Here, go through the 74 resumes and extract keyword list for each field and use that to match resume.

The 1st method saves a lot of time searching the keyword in dataset. But it has 2 main disadvantages:
- `Require large and accurate dataset` -When the appropriate dataset can’t be found, the corresponding keyword list would be less relevant.
- `Require much time to train the model` -When the dataset is large, it would take a lot of time to process it.

After getting an irrelevant keyword list using method 1, I chose to create list by myself.
