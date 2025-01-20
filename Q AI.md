## Feedback System

- Usage: User -> sends input -> AI resposeds -> give the feedback to user (thumb-up thumb-down)
	- Feedback stores on google drive for it can be retrived to prefab 

## System Workflow
	6. DB: stores and export info into server, so dev can be used 
	7. Do pretrain 
	8. We can stores both model into HuggingFace and Googld Drive

## Data preparation
- Data are stores into template 
### Procedure
1. Data Cleaning  (Programming)
2. Divide Data into 2 type Training and Testing
3. Data Cleaning (Manually)
4. Save into db

## Fine-tuning
1. Load model and pull info from HuggingFace db
2. Do data preparation for data training (pythainlp using for do data preparation)
3. Data training -> using text from data preparation 
4. performance test (Ai tools such an perplexity)

## PEFT 
### Parameters
1. r -> curve for learning curve (if more than it had good for learning)
2. target_modules
3. model -> ollama model
4. lora_alpha
5. lora_dropout
6. bias
7. use_gredient_checkpointing
8. random_state
9. use_rslore
10. loftq_config
### Data preparation for training

## Model Testing
- BLEU score: if is comes near to 1, nearer means it good. (using for lang translate)

- perplexity (1-100): nearer to 100, less concise for prediction
 - ROUGE/N-Grams:
	 - 1,2,3: Compared word subsets (1-Grams means 1 word subset, subset words increase by n-grams size.)
	 - L -> compared word length
- Contextual Embedding Metrics
- BRE
	- B precision
	- BLEU recall: nearer to 0, less concise, prediction 
	- F1

## Demo 
1. On this run, we try common question to see if responded seamless or not  (คุณชื่ออะไร)
	1. ชื่ออะไร
	2. อายุเท่าไหร่
	3. CAMT คืออะไร
2. try another common question but this time we change some words for the sentence that not usually used in real word (ชื่ออี้ยัง, มีชื่อเล่นมั้ย ถ้าไม่มีขอตั้งได้มั้ย etc.)
	1. ไม่ขอเรียกน้อง เรียกพี่ฟ้าได้มั้ยจ๊ะ



# Comment
- Some slide from presentation not in the project documentation
- Research 
	- what is N-Grams using for LLM
- ai respond seems like output that had input and output concat together
	- keyword: 
	- solution: 
		- using longest common subsequence method for cutting some part of output that seems likes question by checking if the some part of length is similar to the input.
- 

# Question
1. Why you stores model into HuggingFace and Google Drive? What is the different between and what pro and cons.
2. Why do we need to save pre test model into google drive or db?
	1. Since you can save it directly into hugging face
	2. It may be redundant info since all data are the same
	3. Answer: we need to store it to alternative storage because it can be easier to retrieved for using for dashboard 
1. Who need to be used for statistic? and how these data can be useful
2. training parameter 
3. From the chat template (3 columns), but when using on ollama it got 2 columns why
	- Answer: 
		1. Instruction: context for tell ai to know what it should be responded if input is are similar on the definition 
		2. Input: User input
		3. Output: 
	- Q: What is the different between concatenation or merge from columns
	- Q: Do concat may be for seamless responded more than merging?
	- Q: Do "System" that in the first column is "instruction"?
	- Q: You said that output would that had already done training it should have 2 columns (input, output). But on the present it shows that it got 3 columns (system, input, output)
4. Do training and test dataset are using same method?
5. What is the word prediction?
6. What is useful for word prediction (3-Grams) ?
7. Precision and recall what is the different?
8. How far this project will end like you expected? and what the critical problem that you occur?
	1. Q: How far critical problem will can be fixed?
		1. Answer: 1 Month.
	2. Q: From the company that hired you. what is the sign that you sense that this project may not be complete from what you expected.
	3. Q: Do this technology that you tried to integrated with (meta-verse) are ready for launch to production
	4. Q: How this product that you created would be apply to other product in the future? and how much you seen value from this product.
	5. Q: 