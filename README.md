# Odia-KenLM_MMS
Integrating KenLM and MMS model for Odia language

## Goal
1. Create arpa and lexicon files from given Odia dataset using KenLM
2. Correct arpa file, adding the required annotatins at end of sentence
3. Create lexicon file i.e. splitting words into characters, assisting decoder to decode using words given in lexicon file

4. Download MMS model from source
5. Integarte MMS and KenLM files using decoding arguments

### Issue Ticket
[Issue](https://github.com/Samagra-Development/ai-tools/issues/211)

### KenLM
1. Run [notebook](https://github.com/Jiya126/Odia-KenLM_MMS/blob/Jiya126-patch-1/Copy_of_KenLM_.ipynb) to generate and download '5gram_test.bin' and 'lexicon_test_p.txt' files
2. Use these files in decoding_cmds and run integration of MMS and LM
In this step, we are getting assertion error i.e.
![image](https://github.com/Jiya126/Odia-KenLM_MMS/assets/90051748/d0a7477f-5077-4d33-88bf-9b1b32bf10c3)

#### Approaches tried so far:
1. The error is due to unk_word token present in sentence and thus KenLM model won't be able to score it.
But when getting tgt_dict and word being unk token there, in the Assertion error
![image](https://github.com/Jiya126/Odia-KenLM_MMS/assets/90051748/612d8488-355d-46f0-806f-c90c24146799)


It shows error as 

AssertionError: ହେଊଛି ['ହ', 'େ', 'ଊ', 'ଛ', 'ି', '|'] [18, 10, 3, 44, 8, 4] ['<s>', '<pad>', '</s>', '<unk>', '|', '୍', 'ା', 'ର', 'ି', 'କ', 'େ', 'ବ', 'ନ', 'ତ', 'ସ', 'ପ', 'ୁ', 'ମ', 'ହ', 'ୟ', 'ଲ', 'ଏ', 'ଥ', 'ଦ', 'ୋ', 'ଣ', 'ଇ', 'ଟ', 'ଗ', 'ଯ', 'ଅ', 'ୀ', 'ଜ', 'ଶ', 'ଆ', 'ଷ', 'ଡ', 'ଧ', 'ଭ', 'ଳ', 'ଙ', 'ଚ', 'ଉ', 'ଂ', 'ଛ', 'ଁ', 'ଖ', 'ୂ', 'ଫ', 'ୱ', '଼', 'ଠ', 'ୃ', 'ଓ', '0', '1', '।', 'ଘ', 'ଞ', 'ୌ', '\u200c', '2', 'ଵ', '-', '9', 'ୈ', '5', 'ଃ', '4', '6', '3', '8', 'ଢ', '.', 'a', 'ଝ', '7', 'm', 'n', 'c', 'i', 'e', "'", 'p', 't', '/', 's', 'o', 'd', ',', 'r', ':', 'b', '’', 'l', 'h', 'u', '\u200d', 'ଋ', 'w', 'ଔ', 'ଐ', 'k', 'g', '%', 'ଈ', 'f', '"', 'v', 'z', 'y', 'x', '[', ']', ';', '¥', '‘', '”', '୦', '+', 'õ', '*', 'ୗ', 'j', 'ୄ', '°', '୭', '£', '!', 'q', '$']

The word_indx for this corresponds to  ହେଊଛି 403 ['ହ', 'େ', 'ଊ', 'ଛ', 'ି', '|'] [18, 10, 3, 44, 8, 4]

And we have the word present in our tgt_dict here 

Therefore, the problem could be with reading the dict file.

1. a) Referencing the above problem, there are some characters that are not present in MMS dictionary we are accessing.

   b) KenLM knows how to deal with unk tokens (words here), assigning it probability defined in the arpa file.
   
   c) But MMS deals with the characters present in its dictionary only. {Verify this}

   d) Inspect how are we integrating these models, parallel or sequentially; and what library are we using for that. Inspect that library and function performing that (This function is in mms_infer.py). Check what's happening there and how is it being integrated. Then later, you might go on to check if we should make our own integration function or maybe optimize the area where error is showing in pre-defined function we are using.




#### Latest Inference
The error was with not all characters being present in the dictionary.

Therefore, removed the characters from training data that are not in dict to create [new training data](https://github.com/Jiya126/Odia-KenLM_MMS/blob/Jiya126-patch-1/kenLM%20files/new_lm_train.txt) 
Now, use this data to generate bin and lexicon files and run inference model. This shows no error and models are integrated successfully.
What is left is to check the accuracy of produced transcription.
