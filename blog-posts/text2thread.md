---
description: Machine Learning to Make Non-Fiction more readable
---

# Text2Thread

## The Problem

I can read forum threads for hours \(HackerNews, reddit, etc.\). But I want to spend that time reading real books.

## The Solution

[https://text2thread.herokuapp.com/](https://text2thread.herokuapp.com/)

Make Long-form text more like a thread. Give paragraphs big indentation to indicate how it relates to the text that has come before it.

I created an embedding for each paragraph using the [doc2vec model from Gensim](https://radimrehurek.com/gensim/auto_examples/howtos/run_doc2vec_imdb.html#sphx-glr-auto-examples-howtos-run-doc2vec-imdb-py). 

### Document Tree

I used a tree-like structure with each paragraph as a node. If the node was more similar to the previous node than the next node, the depth of that node was increased and the next node was pushed to that depth. In the other case, I would look to see if there was any node up the tree that it could be branched off of \(more similar than the next node\).

```python
for index, paragraph in enumerate(cleaned_train_corpus[1:-1],start=1):
    sim_prev = model.docvecs.similarity(paragraph.tags[0], paragraph.prev.tags[0])
    sim_next_ = model.docvecs.similarity(paragraph.tags[0], paragraph.next_.tags[0])

    if sim_prev >= sim_next_:
        paragraph.depth +=1
        paragraph.next_.depth = paragraph.depth
    else:
        search_depth = paragraph.depth
        while search_depth > 0 :
            sim_1 = model.docvecs.similarity(paragraph.tags[0], paragraph.prev.tags[0])
            sim_2 = model.docvecs.similarity(paragraph.tags[0], paragraph.prev.prev.tags[0])
            if sim_2 > sim_1:
                search_depth-=1
                
            else:
                paragraph.depth = paragraph.prev.depth
                search_depth =-1
```

## Text Cleaning.

I removed text between brackets, which I took to be footnotes. I replaced all words only mentioned once with a special signifier. I converted everything to lower case and added signifiers denoting uppercases.

```python
def replace_words(word_list):
    new_list = ["xxxbos"]
    for i in word_list:
#         if i[0] == '"':
#             new_list += ["xxxquote"]
        #check if in vocab
        if i in vocab:
            token = i
        else:
            token = "xxxunk"
        if i == i.title():
            new_list += ["xxxcap", token.lower()]
        elif i[-1] == ".":
            new_list += [token[0:-1], ".", "xxxbos"]
        elif i[-1] == ",":
            new_list += [token[0:-1]]
        elif i[-1] == '"':
            new_list += [token[0:-1,"xxxquote"]]
        else:
            new_list +=[i]
    return new_list
```



