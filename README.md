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
![image](https://github.com/Jiya126/Odia-KenLM_MMS/assets/90051748/04d5098a-db14-444f-a5e2-9dfc8f76c638)
#### Approaches tried so far:
1. The error is due to unk_word token present in sentence and thus KenLM model won't be able to score it.
But when getting tgt_dict and word being unk token there, in the Assertion error
![image](https://github.com/Jiya126/Odia-KenLM_MMS/assets/90051748/89949370-6e06-425b-8d8d-f299b8726d12)
It shows error as 

AssertionError: ହ 24 ['େ', 'ଊ', 'ଛ', 'ି', '|'] [10, 3, 44, 8, 4] ['<s>', '<pad>', '</s>', '<unk>', '|', '୍', 'ା', 'ର', 'ି', 'କ', 'େ', 'ବ', 'ନ', 'ତ', 'ସ', 'ପ', 'ୁ', 'ମ', 'ହ', 'ୟ', 'ଲ', 'ଏ', 'ଥ', 'ଦ', 'ୋ', 'ଣ', 'ଇ', 'ଟ', 'ଗ', 'ଯ', 'ଅ', 'ୀ', 'ଜ', 'ଶ', 'ଆ', 'ଷ', 'ଡ', 'ଧ', 'ଭ', 'ଳ', 'ଙ', 'ଚ', 'ଉ', 'ଂ', 'ଛ', 'ଁ', 'ଖ', 'ୂ', 'ଫ', 'ୱ', '଼', 'ଠ', 'ୃ', 'ଓ', '0', '1', '।', 'ଘ', 'ଞ', 'ୌ', '\u200c', '2', 'ଵ', '-', '9', 'ୈ', '5', 'ଃ', '4', '6', '3', '8', 'ଢ', '.', 'a', 'ଝ', '7', 'm', 'n', 'c', 'i', 'e', "'", 'p', 't', '/', 's', 'o', 'd', ',', 'r', ':', 'b', '’', 'l', 'h', 'u', '\u200d', 'ଋ', 'w', 'ଔ', 'ଐ', 'k', 'g', '%', 'ଈ', 'f', '"', 'v', 'z', 'y', 'x', '[', ']', ';', '¥', '‘', '”', '୦', '+', 'õ', '*', 'ୗ', 'j', 'ୄ', '°', '୭', '£', '!', 'q', '$']

And we have the word present in our tgt_dict here 
Therefore, the problem could be with reading the dict file.

2. So, we need to map the dictionary being used here to remove those unk tokens from lexicon file, so that the assertion error doesnot occurs. This is a temporary solution, since here we are taking away the more correct words that could be present in the lexicon and increase accuracy
3. We can expand our dict and then, use any word from lexicon file
4. But first of all, we need to access the dict that is being used here from the source.
We have the dict downloaded from source MMS, but looking to access the one from source code here itself, the tgt_dict mentioned in [flashlight_decoder.py](https://github.com/facebookresearch/fairseq/blob/main/examples/speech_recognition/new/decoders/flashlight_decoder.py)
