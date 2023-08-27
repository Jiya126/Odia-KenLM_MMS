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

### Inference
1. Using new training data and dict, we have [lexicon](kenLM files/new_lexicon_test.txt) and [bin file](kenLM files/new_5gram_test.bin)
2. Use these files to run the inference in [notebook](Copy_of_Fseq_MMS_ASR_Inference_Colab.ipynb)

### Output
1. On inference, the latency is reduced by 4 sec.
2. And the WER (calculated using [jiwer](https://github.com/jitsi/jiwer)) turns out to be 0.795 
