# [Paper Reading 10] To Test Machine Comprehension, Start by Defining Comprehension  
paper link: https://www.aclweb.org/anthology/2020.acl-main.701.pdf  
ACL 2020.  

# Poblem Definition
## [1] Assumption and Goal
**MACHINE READING COMPREHENSION (MRC)**:  
* Old Methods: Ever-more-“difficult” question-answering tasks will ultimately lead to more robust and useful reading comprehension.  
    * questions requiring commonsense reasoning  
    * multihop reasoning  
    * inferences based on a second passage  
    
**Focus on difficulty rather than basic comprehension**

* Proposed Method: Content—what information expressed, implied, or relied on by the passage—systems should comprehend.  

## [2] Novelty and Contribution
* Argue that existing approaches do not adequately define comprehension; they are too unsystematic about what content is tested.
* Present a detailed definition of comprehension—a TEMPLATE OF UNDERSTANDING—for a widely useful class of texts, namely short narratives.

## [3] Promising Application
* Propose a “template of understanding” (ToU) for stories.  

# Method/Technical Summary

## [1] Existing MRC dataset designs
* **Manually written questions:** to have humans—usually crowd workers, but sometimes trained annotators—think of questions about each passage.  
    * open-ended generation process, but absent of stronger guidance.  
    * incorporate trickier twists, but pursue hard questions is to overlook.  
* **Naturally occurring questions:** to find questions “in the wild”, but fail for more complex applications.  
* **Questions from tests designed for humans:** to pull questions from tests written for humans, but the questions they ask only tangentially graze the content of each passage  
* **Automatically generated questions:** 
    * Produce cloze-style questions over news passages by masking out entities from summaries and below-the-fold sentences.
    * Test for multi-hop reasoning by walking a structured knowledge base.
    * Generates short texts and questions from a simple simulation of characters moving around.  
All assumptions about what is worth asking.  

**What is needed: To test reading comprehension
would be to select passages, describe what should be comprehended from them, and design tests for that understanding.**

## [2] Defining deep story understanding

Starts from the content of a passage, which we define as the information it expresses, implies, or relies on.  

* **The case for stories:** 
In many domains will depend on stories in customer complaints, intelligence dispatches, assess whether a lawsuit may be warranted, finds candidates for medical trials, financial news, and many other document types.
* **A ToU for stories:** 
    1. Spatial: Where are entities positioned over time, relative to landmarks and each other? How are they physically oriented? And where do events take place?  
    2. Temporal: What events and sub-events occur, and in what order? Also, for what blocks of that timeline do entities’ states hold true?  
    3. Causal: How do events and states lead mechanistically to the events and states described or implied by the text?  
    4. Motivational: How do agents’ beliefs, desires, and emotions lead to their actions?  
* **Towards a story understanding task:**
    * Approach 1: Annotating ToU answers  
    Trained annotators writing plain-English answers to each ToU question.  
    We conducted a small pilot study on RoU annotation: with the help of 5 annotators, we iteratively crafted guidelines and tested them on 12 stories. And annotators occasionally differed on which causal contrasts to include.  
    
    * Approach 2: Competing to satisfy judges  
    Let humans judge system’s full free-text answers based only on intuitive preferences. Evaluators could still be guided to ask ToU questions thoroughly, but extensive guidelines would not be needed: neither asking questions nor recognizing good answers demands nearly as much specification as stating canonical answers.  

# Experiment Results

## [1] Results
![](https://i.imgur.com/s1XcTXX.png)  

## [2] Discussion
* XLNet performs poorly. This strongly suggests that whatever XLNet is doing, it is not learning the ToU’s crucial elements of world understanding.  
* The system’s performance is brittle, with many correct answers attributable to luck and/or unreliable cues.
* No existing dataset is so much more systematic than RACE.

## [4] Future work
* We have argued for the ToU as a **minimal** standard and a valuable target for MRC. You can refining and perhaps expanding the questions.  

## [3] Question
* How about culture or generation information in the language, can it be comprehanded, too? (just like trying to fit in a special group by chatting)