# Reading Between the Lines: Classifying Idiomatic Phrases for Literal vs Idiomatic Sense

Abstract:

Idioms and popular phrases (मुहावरे) are very vital to any language and the culture. These phrases are very culturally specific, one cannot just translate the phrases directly from one language to another. You need to understand the deeper meaning, history and cultural significance of the phrase. 

When we use Google Translate on the text “यहाँ दिखावा मत करो राहुल, नाक कट जाएगा” it translates to “Don't show off here Rahul, you will get a nose job.” You see, this doesn't make any sense when we use machine translation tools directly onto idioms. The translation was meant to be “Don't show off here Rahul, you’ll be humiliated.” as the phrase “नाक कट जाना” means “to hurt one’s reputation.” 

But the term cannot always be used in its idiomatic meaning, let’s say we have a text “अर्जुन लापरवाही से खेल रहा था। इससे अर्जुन को चोट लगी और उनकी नाक कट गई” here the phrase “नाक कट जाना” (to cut one’s nose) does not imply any use of the idiom, it should simply be translated as “Arjun was playing carelessly. He got hurt and cut his nose.”

In this project, we developed a method to not only identify when an idiomatic phrase is used in any text, but also classify the meaning behind the usage of the phrase as:
a.) Its idiomatic meaning
b.) The literal sense of the words

Introduction:

	Idioms play a huge role in adding depth to the social context of any language. They give you an insight on cultural references, add humor to dialogue and make any simple straight forward text more entertaining. Accurate translation of Hindi Idioms or मुहावरे is very important in order to understand and preserve rich cultural history and nuances in a language and its texts. Many hindi idioms were adopted hundreds of years ago and are still widely used. For instance during the British Raj in India, Indian Freedom Fighters used the code 9-2-11 in order to signal to others that British soldiers were coming their way, they had to escape. It has now become a commonly used phrase “नौ दो ग्यारह होना” (to nine two eleven) means “to evade from a situation.”

	This pipeline aims to identify the presence of idioms in text, and then classify whether the phrases are idiomatically used or literally used. The challenge when working with Hindi phrases is there are many variations of the same words. Words have morphological variations like for the phrase “सिर खाना” (to eat one’s head) words differ based on context, “सिर खा रही”, “सिर खा रहा”, “सिर खा रहे”. Hindi phonetics are based upon “Matra”s that work as vowels in English, but are not their own letter. Thus there are multiple accepted versions of the same word. This leads to high semantic complexity. 

	Using Levenshtein Distance or minimum edit distance technique, we find the closest matching words from the text to the idiom tokens. Largely, the approach is divided into 2 parts: Firstly, processing idioms, tokenizing them and then forming english keywords. The second part includes identifying the idioms present in the text, and classifying them into literal or idiomatic phrases using the keywords.

	There have been several researches conducted on processing and identifying idioms in several Indian languages. These methods are successful in finding the presence of idiomatic phrases in text, but fail to identify if the usage is literal or metaphorical. This paper embarks on exploring the methods to identify the usage sense on these phrases. We use [Idiom token, keyword set] pairs for each idiom to parse. The idiom_tokens and idiom_keywords form the pairs that we use to identify and classify idioms. The list consists of over 350 idioms, some idioms repeated with different variations in spelling. 

	
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

	The idiom token is a tokenized form of the entire idiom excluding stopwords and conjunctions. We also remove “-ना” at the end of many words as it is the English equivalent of “to.” Words like पर, का, में etc. will cause unnecessary matching with text. We also remove the word “होना” as it is used in idioms to describe the situation, but does not provide meaning to the context.
	
We use Regular expression tools in python via Jupyter notebook
	The idiom tokens are translated into English using Google’s Deep Translator forming short phrases. These phrases are then used to form keywords corresponding to the idiom. Keywords are generated by tokenizing the idiom followed by removing stopwords.

[currently working on the writing]

