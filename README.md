# English to Hinglish Translation AI (Llama-2)
Built an english to hinglish translator by fine-tunning Llama-2 LLM. The translated text sounds natural and also convert all the difficult words and phrases in English to Hinglish. The translated text is easy to understand for even a non-native Hindi speaker.

### Final Outputs:
 >English: Definitely share your feedback in the comment section.<br />
 >Hinglish: बिल्कुल, comment section में अपनी feedback साझा करें।


 >English: I was waiting for my bag.<br />
 >Hinglish: मैं अपनी bag का इंतजार कर रहा था।


 >English: Please like and subscribe my youtube channel.<br />
 >Hinglish: कृपया मेरे youtube channel को like और subscribe करें।

##### Results on some sample inputs are inside [outputs.txt](https://github.com/stokome/Hinglish-Translation-AI-llama2/blob/main/output.txt)

### How to use the model:
Run the ipython notebook hinglish_translator_llama2.ipynb in google colab.

There are two types of weights:
- [Custom Weights](https://github.com/stokome/Hinglish-Translation-AI-llama2/tree/main/llama-2-7b-hinglish_weights/custom_dataset_weights):  weights after training on custom dataset (better results than huggingface weights).
- [Huggingface Weights](https://github.com/stokome/Hinglish-Translation-AI-llama2/tree/main/llama-2-7b-hinglish_weights/huggingface_dataset_weights): weights after training on huggingface dataset: [findnitai/english-to-hinglish](https://huggingface.co/datasets/findnitai/english-to-hinglish)

### Methodolgy:
#### Dataset Generation:
Opensource dataset avalilable are not up to the mark for finetunning of LLMs. So I had to create a custom dataset for the purpose of finetunning Llama-2. Dataset is generated with the help of ChatGPT using prompt:
>You are an expert English to Hinglish Translator. The translated text should sound natural and also
convert all the difficult words and phrases in English to Hinglish. The translated text must be able to keep certain words in English to keep the Hindi translation Easy.
\###
>Example:
English: I had about a 30 minute demo just using this new headset
Hinglish:  मुझे सिर्फ ३० minute का demo मिला था नये headset का इस्तमाल करने के लिए
\###
>Generate a dataset of 5 examples for English to Hinglish translation where Hindi words should be in Devanagari and English words should be in English. Use the above example as a reference. Create examples biased towards content creators.

Dataset consist of 79 training examples created by repeating the above prompt. Here knowledge from the [LIMA paper](https://huggingface.co/meta-llama/Llama-2-7b-hf) is applied to generate dataset biased towards content creators as our use-case sample inputs are going to be biased towards them. 

#### Training:
Tranined the Llama-2 model using parameter efficient finetunning algorithm called [QLORA](https://arxiv.org/abs/2305.14314) which allows LLMs learn by adding an adapter between its layers and only change weights inside the adapters without touching weights in other layers. This allows pre-trained models to retain the knowledge gained during pre-training and apply the knowledge gained during finetunning.


### Challanges Faced and Solved :
- Previous state of the art translation models like [T5](https://huggingface.co/findnitai/t5-hinglish-translator) did not perform well on English to Hinglish translation even after fine-tunning on large datasets. So I decided to move forward with a advanced Large Language Model(LLM)  Llama-2 7 billion parameter variant.  

- Trained the Llama-2 model on [findnitai/english-to-hinglish](https://huggingface.co/datasets/findnitai/english-to-hinglish) dataset from huggingface using QLORA a parameter efficient fine tunning which helps to fine-tune LLMs without the fear of catastrophic forgetting (forget the knowledge learned during pre-training)

- Results from above fine-tunning was a major improvement from earlier models like T5 but outputs were still not upto mark (you can view this output in outputs.txt under huggingface dataset section).

- To further improve the outputs of the translation, applied the LIMA paper according to which fine tunning on small amount of  well curated training examples can outperform finetunning on large amount of data for a specific use-case. So created a custom dataset of 79 training examples biased around content creators.

- Finetuning on the custom dataset resulted in the model to give highly accurate hinglish translations for english text biased towards content creators.


#### References:
[Llama 2](https://huggingface.co/meta-llama/Llama-2-7b-hf)

[LIMA paper (Less Is For More Aligment)](https://arxiv.org/abs/2305.11206)

[QLORA parameter efficient finetunning](https://arxiv.org/abs/2305.14314)