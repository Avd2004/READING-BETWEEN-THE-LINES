Reading Between the Lines: 

Classifying Idiomatic Phrases for Literal vs Idiomatic Sense

Abstract:

Idioms and popular phrases (मुहावरे) are very vital to any language and the culture. These phrases are very culturally specific, one cannot just translate the phrases directly from 	one language to another. You need to understand the deeper meaning, history and cultural significance of the phrase. Several times the idiom or the phrases are not used in its metaphoric meaning, they are used literally to describe a scene. For example the English phrase "to break the ice” means to reduce the tension or awkwardness in a conversation; “to throw in the towel” translates to accepting defeat. But whenever we come across these phrases of course it doesn't always mean in their idiomatic sense, it might just be used literally. Someone might actually be breaking the ice, throwing in a towel, getting a bad apple or hitting the sack. Though the occurrence of these phrases in their literal sense are scarce, we need an efficient way of classifying the type of usage of the idioms. In Hindi these idiomatic phrases are called मुहावरे and an essential part of day-to-day conversations, unlike English, the overlapping of literal and metaphoric usage of idioms is far more frequent. Thus to aid in machine translation of Hindi text, we have developed a pipeline that identifies the presence of commonly used Hindi phrases and then concludes whether the usage of the idiom was literal or idiomatic.

Keywords: Idioms, Natural language processing, Idiom Identification

Introduction:

	Idioms play a huge role in adding depth to the social context of any language. They give you an insight on cultural references, add humor to dialogue and make any simple straight forward text more entertaining. Accurate translation of Hindi Idioms or मुहावरे is very important in order to understand and preserve rich cultural history and nuances in a language and its texts. Many hindi idioms were adopted hundreds of years ago and are still widely used. For instance during the British Raj in India, Indian Freedom Fighters used the code 9-2-11 in order to signal to others that British soldiers were coming their way, they had to escape. It has now become a commonly used phrase “नौ दो ग्यारह होना” (to nine two eleven) means “to evade from a situation.”
When we use Google Translate on the text “यहाँ दिखावा मत करो राहुल, नाक कट जाएगा” it translates to “Don't show off here Rahul, you will get a nose job.” You see, this doesn't make any sense when we use machine translation tools directly onto idioms. The translation was meant to be “Don't show off here Rahul, you’ll be humiliated.” as the phrase “नाक कट जाना” means “to hurt one’s reputation.” 

But the term cannot always be used in its idiomatic meaning, let’s say we have a text “अर्जुन लापरवाही से खेल रहा था। इससे अर्जुन को चोट लगी और उनकी नाक कट गई” here the phrase “नाक कट जाना” (to cut one’s nose) does not imply any use of the idiom, it should simply be translated as “Arjun was playing carelessly. He got hurt and cut his nose.”

	This pipeline aims to identify the presence of idioms in text, and then classify whether the phrases are idiomatically used or literally used. The challenge when working with Hindi phrases is there are many variations of the same words. Words have morphological variations like for the phrase “सिर खाना” (to eat one’s head) words differ based on context, “सिर खा रही”, “सिर खा रहा”, “सिर खा रहे”. Hindi phonetics are based upon “Matra”s that work as vowels in English, but are not their own letter. Thus there are multiple accepted versions of the same word. This leads to high semantic complexity. 

	Using Levenshtein Distance or minimum edit distance technique, we find the closest matching words from the text to the idiom tokens. Largely, the approach is divided into 2 parts: Firstly, processing idioms, tokenizing them and then forming english keywords. The second part includes identifying the idioms present in the text, and classifying them into literal or idiomatic phrases using the keywords.

	There have been several researches conducted on processing and identifying idioms in several Indian languages. These methods are successful in finding the presence of idiomatic phrases in text, but fail to identify if the usage is literal or metaphorical. This paper embarks on exploring the methods to identify the usage sense on these phrases. We use [Idiom token, keyword set] pairs for each idiom to parse. The idiom_tokens and idiom_keywords form the pairs that we use to identify and classify idioms. The list consists of over 350 idioms, some idioms repeated with different variations in spelling. 

	
Examples:	



	This study aims to help improve the translations on commonly used Hindi phrases with the correct contextual awareness in order to aid machine translation of literature, online text messages, social media content and many other places that can help increase the inclusivity and diversity of knowledge and communication over the internet. 

The main structure and steps involved ares:
Pre-processing idioms and forming idiom_tokens and idiom_keyword pairs for each idiom
Tokenize the text that needs to be identified for idioms into sentences.
Use Levenshtein distance for string matching of the idioms in the text.
If a close match is found, note the index of the idiom that matched with the text.
Store the matched sentence along with the previous sentence as ‘context.’
Translate the context in english to ensure only the meaning is passed, not the idiom.
Obtain the keyword set of the idiom from its index.
Using zero shot classification, classify if the context relates to the keyword or not.
a.) If the context is related to any of the keywords, then the phrase was used in its literal sense.
		b.) If the context is unrelated to any of the keywords, it is used in its idiomatic meaning.
Methodology:

Pre-processing of Idioms:

	The idiom token is a tokenized form of the entire idiom excluding stopwords and conjunctions. We also remove “-ना” at the end of many words as it is the English equivalent of “to.” Words like पर, का, में etc. will cause unnecessary matching with text. We also remove the word “होना” as it is used in idioms to describe the situation, but does not provide meaning to the context. We use Regular expression tools to replace and remove these characters. Eg. the idiom “ईद का चाँद होना” is reduced to “ईद चाँद,” “ऊँट के मुँह में जीरा” becomes “ऊँट मुँह जीरा.”

	After reducing the idioms, they are translated into English using Google’s Deep Translator forming short english phrases. Creating an array of english translations of each idiom. We use the NLTK library to identify and eliminate stop words. After that we manually select the keywords to be presented. These phrases are then used to form keywords corresponding to the idiom. Keywords are generated by tokenizing the idiom followed by removing stopwords. Each idiom has 0 ,1, or 2 keywords. 0 keywords for the idioms that are very unlikely to be used in a literal sense. 1-2 keywords for the idioms that can have scenarios in which the phrase would be likely to be the literal words. The selection of keywords from the reduced words of translation is performed manually. After that we convert each set of keywords into an array of sets for each idiom. 




Identifying the idioms present in text

	Now we have 2 components to process: a [idiom_tokens, idiom_keywords] pair and the text in which we need to search for the presence of idioms. Applying normal string matching cannot be directly applied here. The semantic variations, morphologies in Hindi verbs, and various versions of accepted spellings make it a difficult task to search for the phrases in text. To overcome this issue, we use Levenshtein Distance. Levenshtein Distance measures the difference between two strings and returns a numeric value called the Minimum Edit Distance. 

	The Levenshtien distance between two Hindi strings d(i,j) where i and j are characters from wods a and b. The conditions for this measure are:
If a character is empty, d = length of the other string.
If the last characters of the substrings are the same, d =  same as the distance between the substrings without the last character.
If the last characters are different, consider the minimum distance d  after an insertion, deletion, or substitution.
Example of using Levenshtein Distance for 2 Hindi strings:


We use minimum similarity of 2 words as a limiting factor for matching i.e. at least 2 words from the idiom tokens should match with the text of the sentence. Due to variations in spellings of many words in Hindi, we cannot use the entire idiom for matching. But using this approach has its own limitations as some words of the idiom that are present in sentence but not used as a phrase leads to necessary matching. We reduced the ratio of such false positives significantly by tokenizing idioms. Additionally we extend the condition that the order of usage of matching tokens should be the same in text as found in the idiom.


Processing and Translating the Context

	The Matched sentence along with the previous sentence, now form a context. The context sentences help us determine the type of usage of idioms if present in the text. We need to remove the common words from this formed context as the words will be translated and potentially become the keywords itself, so to prevent misleading conclusions, we eliminate the overlapping words. For eg. the context “वो काफी बेहूदा बाते करते रहता है। मैंने उसे कहा भाई तेरी अक्ल चरने गई है क्या” becomes “वो काफी बेहूदा बाते करते रहता है। मैंने उसे कहा भाई तेरी गई है क्या” by removing the matched tokens “अक्ल चरने जा.” 

The resulting context is then converted to English using Google’s Deep Translator. We translate the context to English for 2 important reasons. 
We are purely extracting the meaning of context thus eliminating any entendres and hidden meaning that may lead to wrongful classification.
Availability of accurate text classifiers in English. Unlike Hindi, there are precise Large Language Model based classifiers for English, like the one we have used.

Classifying: Literal or Idiomatic usage of phrase

	We are performing classification based on the condition- is the context related to the idiom keyword. If yes, then it is likely that the idiom was used in its literal sense. If no, then we can conclude there was no relation between the context and keyword, thus the phrase was idiomatically used. The    method we use to classify is called Zero-shot Classification. It is a classification method where the model is trained on a set of labeled examples, but after training it can perform classification in unseen labels.
As there is not a good collection trained model selection available, we translate our hindi context to english. Essentially we are only translating the meaning and not the idiom, using translated context does not hamper our cause. We feed the corresponding keyword to the idiom that matched into the classifier as label. The Zero shot classifier using the model facebook bart-large-mnli, we check if the context is related to the label as keyword or not. For the case of 0 keywords, it directly classifies as not related.
For multiple keywords, even if one of the keywords is related, the phrase is related. The result is then presented. If is_related = True, then the phrase is used in its literal sense, if is_related = False, the phrase is used idiomatically.







Experiment:

Setup:
Text: A total of 200 sentences (generated using ChatGPT)
	100 auto generated hindi sentences that contain idioms.
	100 auto generated hindi sentences without any idioms.
Idioms: 310 idioms as whole idioms, idiom_tokens and idiom_keywords

Part A: Idioms Identification: 

TP: Idiom Present in text and match found
TN: No idiom Present in text and no matches found
FP: Idiom not present but matched OR Wrong idiom matched
FN: Idiom present but not matched

Initial Iteration: Using unedited whole idioms directly

TP: 61 (30.5%)
TN: 86 (43%)
FP: 38 (19%)
FN: 15 (7.5%)




Actual Positive
Actual Negative
Predicted Positive
TP: 61 (30.5%)
FP: 38 (19%)
Predicted Negative
FN: 15 (7.5%)
TN: 86 (43%)


Accuracy: 56.54%
Precision: 61.62%
F1 Score: 51.91%

We realize that the majority of False Predictions were due to matching of stopwords with the text thus resulting in incorrect matching. To counter this we tokenize the idioms, remove stopwords and include different spelling versions of some words that were used in idioms like “अंगूठा ” and “अँगूठा.”


Final Iteration: Using tokenized idioms and different spelling variants of same words

TP: 78 (39%)
TN: 98 (49%)
FP: 2 (1%)
FN: 18 (9%)



Actual Positive
Actual Negative
Predicted Positive
TP: 78 (39%)
FP: 2 (1%)
Predicted Negative
FN: 18 (9%)
TN: 98 (49%)



Accuracy: 89.8%
Precision: 97.5%
F1 Score: 88.64%


Part B: Phrasal Classification: 

	Given that an idiom is present and identified correctly, we need to classify the usage of the phrase. We use 55 sentences with a mix of idiomatic and literal use of phrases.

TP: Idiomatic use is classified as idiomatic
TN: Literal use is classified as literal
FP: Idiomatic use is classified as literal 
FN: Literal use is classified as idiomatic

Initial Iteration: Using auto translated keywords and classifier with default threshold

TP: 14 (24.5%)
TN: 8 (14.5%)
FP: 1 (1.8%)
FN: 32 (58.1%)




Actual Positive
Actual Negative
Predicted Positive
TP: 14 (24.5%)
FP: 1 (1.8%)
Predicted Negative
FN: 32 (58.1%)
TN: 8 (14.5%)


Accuracy: 40.0%
Precision: 93.33%
F1 Score: 45.9%

Most of the wrongful classifications were due to distant vague relations between some words. As zero shot classification votes onto the majority of the two labels, say for an idiom the keyword is “eye” so if the probability of related to eye is higher that not related to eye, the text will be classified as related. But this is not what we need; we need direct correlation between the word and text, so we experiment with different threshold values for the probability to qualify as related.

Final Iteration: Using modified keywords after manual selection and adding threshold limits onto classifier

TP: 6 (10.9%)
TN: 39 (70.9%)
FP: 8 (14.5%)
FN: 2 (3.6%)



Actual Positive
Actual Negative
Predicted Positive
TP: 6 (10.9%)
FP: 8 (14.5%)
Predicted Negative
FN: 2 (3.6%)
TN: 39 (70.9%)


Accuracy: 81.82%
Precision: 42.86%
F1 Score: 54.55%


Dataset:
For extraction and collection of idioms:
Online NCERT Books for Class 10:
Use web-scraping tools like Beautiful soup to extract all the text from the website.
Remove all punctuation signs, hyphens and numbers.
Separate the idiom phrases from the rest of the text given as meaning and examples of usage for the idiom.
Convert the resulting list into a csv file ready for pre-processing

Zero Shot classifier:
We use the Facebook Bart Large model that uses the MNLI(Multi-genre Natural Language Inference).
MNLI is a collection of 4.33 Lakh or 433k annotated pairs of textual information.
It is a crowd sourced project modeled on SNLI corpus.

Testing data:
For testing of the accuracy of the pipeline we used auto generated sentences:
Using Hindi sentence generation in ChatGPT
100 sentences that used hindi idioms
100 random hindi sentences that did not have any idioms


Literature Review/Related Work:
Identification of Idioms in Computational Linguistics
There have been several approaches to identify the presence of idioms in a text. For instance, Fente et al. employ the word2vec technique to detect idiomatic expressions in Amharic. Similarly, Priyanka et al. utilize the Hindi WordNet for the identification of idioms in Hindi.
Challenge of String Identification in Hindi
Hindi, being a morphologically complex language, poses unique challenges for idiom identification. Jain et al. conclude into the preprocessing of Hindi text necessary for effective pattern matching, which is crucial for accurately identifying idiomatic expressions in Hindi.
Contextual Classification of Usage
Tedeschi et al. explore a combined approach that relies on a minimum threshold for identification and the usage dependence of idiomatic phrases. This method ensures that idioms are not only identified but also classified based on their contextual usage.
Zero-shot Classification for Text Labels
Assigning a given text to a label, especially when prelabeled Hindi idiom data is unavailable, is a challenging task. Yin et al. discusses various techniques and benchmarks for zero-shot text classification, providing insights into handling texts with no prior labels.
Large Language NLP Models
The development and application of large language models have significantly advanced natural language processing tasks. Lewis et al. showcase improvements in text generation, translation, and comprehension tasks with their pre-trained BART model.
References
Abebe Fente, S Gebeyehu, "Automatic Idiom Identification Model for Amharic Language."
Priyanka,  RMK. Sinha, "A System for Identification of Idioms in Hindi."
S Tedeschi, F Martelli, R Navigli, "ID10M: Idiom Identification in 10 Languages."
L Jain,P  Agrawal, "Text Independent Root Word Identification in Hindi Language Using Natural Language Processing."
W Yin, J Hay, D Roth, "Benchmarking Zero-shot Text Classification: Datasets, Evaluation, and Entailment Approach."
M Lewis, Y Liu, N Goyal, "BART: Denoising Sequence-to-Sequence Pre-training for Natural Language Generation, Translation, and Comprehension."
Levenshtein Distance
Google deep translator
Tokenization
NCERT Hindi Book
Sentence generation using chatgpt
Facebook bart large Multi-NLI



