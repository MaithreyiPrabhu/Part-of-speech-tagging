# Part-of-speech-tagging
Natural language processing (NLP) is an important research area in artificial intelligence, dating back to at least the 1950â€™s. A basic problems in NLP is part-of-speech tagging, in which the goal is to mark every word in a sentence with its part of speech (noun, verb, adjective, etc.). This is a first step towards extracting semantics from natural language text. For example, consider the following sentence: â€œHer position covers a number of daily tasks common to any social director. Part-of-speech tagging here is not easy because many of these words can take on different parts of speech depending on context. For example, position can be a noun (as in the above sentence) or a verb (as in â€œThey position themselves near the exitâ€). In fact, covers, number, and tasks can all be used as either nouns or verbs, while social and common can be nouns or adjectives, and daily can be an adjective, noun, or adverb. The correct labeling for the above sentence is:

```
Her position covers a number of daily tasks common to any social director
DET  NOUN     VERB DET NOUN  ADP ADJ  NOUN   ADJ   ADP DET  ADJ    NOUN

```
Fortunately, statistical models work amazingly well for NLP problems. Consider the Bayes net shown in Figure 1(a). This Bayes net has random variables S = {S1,...,SN} and W = {W1,...,WN}. The Wâ€™s represent observed words in a sentence. The Sâ€™s represent part of speech tags, so Si âˆˆ {VERB, NOUN, . . .}. The arrows between W and S nodes model the relationship between a given observed word and the possible parts of speech it can take on, P(Wi|Si). (For example, these distributions can model the fact that the word â€œdogâ€ is a fairly common noun but a very rare verb.) The arrows between S nodes model the probability that a word of one part of speech follows a word of another part of speech, P(Si+1|Si). (For example, these arrows can model the fact that verbs are very likely to follow nouns, but are unlikely to follow adjectives.)

Data. 

## Algorithm Used

```
Part of Speech tagging is achieved by using the following algortihms:
 1) Simplified Model
 2) Viterbi Algorithm
 3) Max Marginal(using Gibbs Sampling)

 TRAINING the DATA : We process the given training data, and learn the probabilities we would be needing to implement
 the algorithms mentioned above.
 The probabilities which we calculate using this process are :
 1) The probability that a particular speech comes as the first speech in a sentence
 2) The probability that a particular speech follows a given speech
 3) The probability of word given speech

 We use the above mentioned probabilities for our algorithms to follow.

 1) Simplified Model : The algorithm seems to be one of the most interesting ones, because being so simple, it returns more
 than 90% accuracy for us. In this algorithm we follow basics of Bayes. Procedure explained below :
     i) For each word in the sentence, we check for each speech and select the best among them and consider each word to be
     independent. From the training the data, we have the probability of word given speech which we utilize here.
     ii) Once selected a word, we comapare the probability for each speech given this word and multiply it with the
     probability of the speech. The max of this seems to be best suited result for the given word (because we consider
     them to be independent)

 Assumptions taken:
 If a word is not to be found in the training data, we were not able to get word given speech probability for that
 word. After experimenting with all the speech tags available, we found out that if we assign it as a noun, we get the
 best results. (which seems kind of obvious, because most probably any given data set could have different nouns.
 
 2) VITERBI or MAP : To find the best tag sequence, given sentence (or a word sequence), we need to compare all tag sequences.
 Viterbi allows us to do this dynamically and faster.
 Procedure followed while implementing the viterbi  explained below.
     i)  For the first word, the Viterbi coefficient doesnt depend on any transition but two probabilities.
     The probability that the speech comes first in a sentence and obviously,
     the probability of word given speech. We follow a matrix approach here where we maintain a forward matrix and store max   values or called Viterbi Coefficients.
     Thus, we have our first row of the forward matrix here.
     ii) For the second word, we use three probabilities. Emission probability, Transition probability and previous viterbi coefficient. We compute this for each speech tag for the word using viterbi coefficients for each speech tag. For any given particular speech, we take the max of the (transition probability from a speech * previous vierbi coefficient of it) an
     multiply it with the emission.
     iii) We conitnue doing this procedure for all words, and keep filling the forward matrix used by us. Also, for the backtrack, we also maintain that at each point, which previous
     Viterbi coefficient was used, which helps us determine the best path(or the best tag sequence) at the end.

 3) MCMC Gibbs Sampling and MAX Marginal: In gibbs sampling we generate samples. For each sample generation we change one observed value and keeping all observed value as constant and we get 12 probabilities for that observation Now after normalizing the 12 probabilities each corresponding to a particular part of speech for the observed value which we are changing . Now we calculated cumulative sum. A cumulative sum is a sequence of partial sums of a given sequence. For example, the cumulative sums of the sequence {a,b,c,...}, are a, a+b, a+b+c, ... Now we take a random value between 0 to 1 and see
 in which range it fall and for the particular range it fall we assign the appropriate part of speech for it. Similarly
 we do for all the words to produce a single sample. For MAX marginal, we generate 500 samples and we calculate the marginal
proababilities for each tag and return the best tag with highest probability for a particular word. We then return this
 tag for each word with its probability calculated over each the total number of samples generated.
 We have created a table P(Sn/Sn-1,S1) for the calculation of Gibbs sampling in order to sample values from the posterior distribution.
 P(S1) is proportional to P(S1)*P(S2/S1)*P(W1/S1)*P(Sn/Sn-1,S1)
 P(Sn) is proportional to P(Wn/Sn)*P(Sn/Sn-1,S1)
 P(Sn-1) is proportional to P(Wn-1/Sn-1) *P(Sn/Sn-1,S1) *P(Sn-1/Sn-2)
 

```


## Sample Output

```
./label.py bc.train bc.test

 ==> So far scored 2000 sentences with 29442 words.
                   Words correct:     Sentences correct: 
   0. Ground truth:      100.00%              100.00%
         1. Simple:       93.95%               47.60%
            2. HMM:       95.71%               58.10%
        3. Complex:       94.62%               51.45%
```



