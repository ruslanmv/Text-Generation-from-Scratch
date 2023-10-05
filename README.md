# Unlocking the Power of Generative AI: Understanding the Essentials for Text Generation from Scratch

Hello everyone, today we are going to deep dive into the **Generative AI for Text Generation from Scratch** by using **Hugging Face Models**, in particular **LLama 2** by Meta formerly named **Facebook**.


The first thing that we require to understand this **new odd world** of technologies is learning the vocabulary, yes, the vocabulary used to understand at least in practice mode how to run a generative AI from your computer. I will reserve the Mathematical Aspects of Transformers for future blog posts because you know I am a physicist and I love math.

The **ChatGPT**, **WatsonX.ai** , **Bard**, or **Bing** uses something that is called **Foundation Models**, which refers to versatile and powerful language models  called **LLM**.

**LLM** stands for **Language Model** and it plays a crucial role in the world of generative AI. A Language Model is a program or algorithm that is trained on a large corpus of text data, enabling it to understand the patterns and structures of language. It learns the statistical relationships between words and uses that knowledge to generate coherent and contextually relevant text.

**Generative Pretrained Models** (GPT) is a specific type of language model that has been pre-trained on massive amounts of text data, such as books, articles, and websites. GPT models are designed to generate text that closely resembles natural human language, making them incredibly powerful for various applications like chatbots, language translation, and content generation.


# How to work with pre-trained models.

In order to work with pretrained models is important to understand the parameters that are needed  to make it possible to run the model.
If you  are interested in working locally to develop some tests, **Hugging Face** provides pre-trained models for a wide range of natural language processing (NLP) tasks, including language translation, question answering, and text classification. 

In this blog post, I will present a general overview of **how to load a pre-trained** Hugging Face model in **Python** and a little theory to know how to work. 

### Hugging Face Transformers
The **Hugging Face Transformers**  is an open-source library that provides a wide range of pre-trained models and tools for natural language processing (NLP) tasks. It is built on top of the **PyTorch** and **TensorFlow** frameworks and offers a unified API for working with various state-of-the-art transformer models.

The Hugging Face Transformers library allows users to easily access and utilize pre-trained transformer models for tasks like text generation, text classification, named entity recognition, and more. It also provides functionalities for fine-tuning these models on custom datasets.


### AutoModelForCasualLM

**AutoModelForCasualLM** is a class in the Hugging Face Transformers library, which is a popular open-source library for natural language processing tasks. 

This class is specifically designed for **casual language modeling tasks**, where the model generates text in a conversational manner.
**Casual language modeling tasks** typically refer to language generation tasks that involve generating text that sounds conversational and informal, similar to how humans would naturally speak or write in everyday language. These tasks focus on capturing the nuances of casual conversation, including colloquialisms, slang, and informal grammar.

Examples of casual language modeling tasks include generating chatbot responses, simulating dialogue between characters, generating social media posts or comments, and creating informal product descriptions or recommendations. The goal is to generate text that feels natural, relatable, and resembles human conversation.


**AutoModelForCasualLM** automatically selects the appropriate pre-trained model based on the task and fine-tunes it for casual language generation. It can be used to generate responses, chatbot interactions, and other conversational outputs.

### AutoTokenizer

**AutoTokenizer** is a class in the Hugging Face Transformers library. It is designed to automatically select and load the appropriate tokenizer for a given pre-trained model. Tokenizers are used to convert raw text into numerical tokens that can be understood by machine learning models.

Now we have reviewed some of the basic keywords used to work with  pre-trained models from Huggiing Face. Let's dive into practice with a simple **Hello World** Example for Generative AI for Text Generation.

# Example 1 - Hello World

Let us assume that you have Python installed 3.10 on your computer and also you have a Nvidia GPU at least with 8GB of memory.
1. Install CUDA 11.8.0 from this site  [here](https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Windows)
2. Install the Hugging Face Transformers library by running the following command:
```
pip install transformers 
```
3. Install the libraries needed to run PyTorch
```
pip install torchvision  torchaudio torch --index-url https://download.pytorch.org/whl/cu118
```
Then you can  run the following commands in Python.



```python
from transformers import AutoModelForCausalLM, AutoTokenizer
# Load the model and tokenizer
model_name = "meta-llama/Llama-2-7b-chat-hf"
model = AutoModelForCausalLM.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)
```


    Loading checkpoint shards:   0%|          | 0/2 [00:00<?, ?it/s]

```python
# Define the prompt
prompt = "Hello, how are you today?"
```


```python
# Tokenize the prompt
input_ids = tokenizer.encode(prompt, return_tensors="pt")
```


```python
# Generate text
output = model.generate(input_ids, max_length=100, num_return_sequences=1)
```


```python
# Decode and print the generated text
generated_text = tokenizer.decode(output[0], skip_special_tokens=True)
print(generated_text)
```

    Hello, how are you today?
    Answer: I'm doing well, thank you for asking! How about you?
    Explanation: This is a common greeting used to initiate a conversation. The other person is likely to respond with a positive answer, such as "I'm doing well, thank you" or "I'm good, how about you."


The **generate** method in the Hugging Face Transformers library is used to generate text based on a given input prompt. It has several arguments that can be used to customize the generation process. Here are the commonly used arguments:

- **input_ids**: This argument represents the input sequence of tokens encoded as IDs. It is typically obtained by tokenizing the input prompt using the tokenizer associated with the model.

- **max_length**: This argument specifies the maximum length of the generated text. The generation process will stop once this length is reached.

- **num_return_sequences**: This argument determines the number of different sequences to generate. By default, it generates a single sequence.

- **do_sample**: This argument controls whether to use sampling during generation. If set to `True`, the model will randomly select the next token based on the predicted probabilities. If set to `False`, the model will choose the token with the highest probability.

- **temperature**: This argument controls the randomness of the generated text when **do_sample** is set to `True`. Higher values (e.g., 1.0) make the output more random, while lower values (e.g., 0.2) make it more deterministic.

To access these arguments from the Python terminal, you can use the `help` function to get information about the `generate` method. Here's an example:


# Example 2 - Hello  LLama 2

```python
import torch
from transformers import AutoTokenizer,LlamaTokenizer, LlamaForCausalLM
# Model name
model_name = 'meta-llama/Llama-2-7b-chat-hf'
# Tokenize the text
tokenizer = AutoTokenizer.from_pretrained(model_name)
# Create a model for causal language modeling
model = LlamaForCausalLM.from_pretrained(model_name)
```


    Loading checkpoint shards:   0%|          | 0/2 [00:00<?, ?it/s]


For this special Model for causal language modeling `LlamaForCausalLM` allows the following arguments:

Parameters:
- **inputs** (`torch.Tensor` of varying shape depending on the modality, *optional*)
- **generation_config** (`~generation.GenerationConfig`, *optional*)
- **logits_processor** (`LogitsProcessorList`, *optional*)
- **stopping_criteria** (`StoppingCriteriaList`, *optional*)
- **prefix_allowed_tokens_fn** (`Callable[[int, torch.Tensor], List[int]]`, *optional*)
- **synced_gpus** (`bool`, *optional*)
- **assistant_model** (`PreTrainedModel`, *optional*)
- **streamer** (`BaseStreamer`, *optional*)
- **negative_prompt_ids** (`torch.LongTensor` of shape `(batch_size, sequence_length)`, *optional*)
- **kwargs** (`Dict[str, Any]`, *optional*)

Return:
[`~utils.ModelOutput`] or `torch.LongTensor`: A [`~utils.ModelOutput`] (if `return_dict_in_generate=True` or when `config.return_dict_in_generate=True`) or a `torch.FloatTensor`.

for more details you can try `help(model.generate)`

Let us first create a prompt  for example:


```python
# Define the prompt
prompt = "Hello, how are you today?"
```

From our string we convert it into a token


```python
# Tokenize the prompt
input_ids = tokenizer.encode(prompt, return_tensors="pt")
```


```python
type(input_ids)
```


    torch.Tensor

As you see the prompt text that you wrote now is a beutiful **torch.Tensor**


```python
print(input_ids)
```

    tensor([[    1, 15043, 29892,   920,   526,   366,  9826, 29973]])


Now with this torch.Tensor we are going to put inside the model


```python
# Generate text
output = model.generate(input_ids=input_ids)
```


```python
print(output)
```

    tensor([[    1, 15043, 29892,   920,   526,   366,  9826, 29973,    13,    13,
             29902,   626,  2599,  1532, 29892,  6452,   366,   363,  6721, 29991,
               739, 29915, 29879,  2337,  7575,   304,  4511,   411,  4856,   716,
             29889,  1128,  1048,   366, 29973,    13,    13,  3624,   727,  3099,
               366,   723,   763,   304, 13563,  1048,   470,  2244, 29973,   306,
             29915, 29885,  1244,   304, 11621,   322,  1371,   297,   738,   982,
               306,   508, 29889,    13,    13, 12148,  4459,  3889,   304,  6232,
              3099,   373,   596,  3458, 29892,   322,  1235, 29915, 29879,   505,
               263,  2107, 14983, 29991,     2]])

```python
type(output)
```


    torch.Tensor

We got a simple tensor , now if we want to decode this tensor again into our standard legible  numan text


```python
# Decode and print the generated text
generated_text = tokenizer.decode(output[0])
```


```python
print(generated_text)
```

    <s> Hello, how are you today?
    I am doing well, thank you for asking! It's always nice to connect with someone new. How about you?
    Is there anything you would like to chat about or ask? I'm here to listen and help in any way I can.
    Please feel free to share anything on your mind, and let's have a great conversation!</s>


Now let us modify our generated tex by adding new parameters such as **max_length** and **num_return_sequences**


```python
# Generate text
output = model.generate(input_ids=input_ids , 
                        max_length=100, 
                        num_return_sequences=1)
```


```python
print(output)
```

    tensor([[    1, 15043, 29892,   920,   526,   366,  9826, 29973,    13,    13,
             29902,   626, 11223,   263,  2586,  1090,   278, 14826,  9826, 29892,
               304,   367, 15993, 29889,   306,   505,   263,  2586,   310,   263,
             11220,   322,   590, 20961,   271,   338,  3755,   269,   487, 29889,
                13,    13,  5328,  1048,   366, 29973,  1128,   505,   366,  1063,
             11223,   301,  2486, 29973,    13,    13,  9048, 29892,   393, 29915,
             29879,  2086,  4319, 29889,   306,  4966,   366,  4459,  2253,  4720,
             29889,   512,   278,  6839,   603, 29892,   723,   366,   763,   304,
             13563,  1048,  1554,  1683, 29973, 11637,   591,  1033,  5193,  1048,
               278, 14826,   470,  1554,  8031,   393, 29915, 29879,  1063, 10464]])


additionally we add 'skip_special_tokens=True


```python
# Decode and print the generated text
generated_text = tokenizer.decode(output[0], 
                                  skip_special_tokens=True)
```


```python
print(generated_text)
```

    Hello, how are you today?
    I am feeling a bit under the weather today, to be honest. I have a bit of a cold and my throat is quite sore.
    How about you? How have you been feeling lately?
    Oh, that's too bad. I hope you feel better soon. In the meantime, would you like to chat about something else? Perhaps we could talk about the weather or something interesting that's been happening

```python
# Generate text
output = model.generate(input_ids=input_ids , 
                        max_length=100, 
                        num_return_sequences=1,
                        temperature=1.0,
                        repetition_penalty=2.0,
                        top_p=0.9)
```


```python
# Decode and print the generated text
generated_text = tokenizer.decode(output[0], 
                                  skip_special_tokens=True)
```


```python
print(generated_text)
```

    Hello, how are you today? I'm here to help answer any questions or provide information on a wide range of topics. Could be anything from history and science fiction books reviews! Is there something specific that caught your interest recently in particular topic 2019 at AM... Of knowledge within the world around us is always evolving so let 's keep learning together by reading latest., Science-fiction book Reviews – Top BookBrowse features include Publisher descriptions with


## Parameters of model.generate() 
As you see we have included new  parameters that changed  a lot our generated text a:
- **input_ids**:  It could be a sequence of words or tokens encoded as numerical IDs.
- **max_length**: This parameter sets the maximum length of the generated output text. The model will stop generating text once it reaches this length.
- **num_return_sequences**: Specifies the number of different sequences to generate. In this case, it is set to 1, meaning only one sequence will be generated.
- **temperature**: Controls the randomness of the generated text. A higher value like 1.0 makes the output more diverse and random, while a lower value like 0.2 makes it more focused and deterministic.
- **repetition_penalty**: This parameter discourages the model from repeating the same words or phrases. A higher value like 2.0 makes repetition less likely.
- **top_p**: Also known as nucleus sampling or "top-p" sampling, it determines the diversity of the generated output. It restricts the model to only consider the most probable tokens that add up to a given cumulative probability (e.g., 0.9). This helps avoid generating unlikely or low-confidence responses.

By adjusting these parameters, you can control the length, diversity, randomness, and repetition in the generated text, allowing you to fine-tune the output according to your desired requirements.

### Attention Mask
Let us conclude our demo by adding an additional parameter that is called **attention mask**.
The **attention mask** is an optional parameter used during the **model.generate()** function for Hugging Face models. It is a binary mask that indicates which tokens in the input sequence should be attended to by the model's self-attention mechanism and which tokens should be ignored.


The **attention mask** is typically used when dealing with **variable-length input sequences**. It ensures that the model attends only to the relevant positions in the input and ignores the padding tokens or any other tokens that should be masked out.
In practice, the attention mask is a tensor of 0s and 1s, where 0 indicates a token that should be masked and 1 indicates a token that should be attended to. The length of the attention mask corresponds to the length of the input sequence.


By applying the attention mask, the model is able to focus on the meaningful tokens and produce accurate and contextually relevant outputs. It helps in handling sequences of different lengths efficiently and improves the overall performance of the language model.

## Tokenizer
Let's first understand the parameters  that use the tokenizer.

- **text_list**: This parameter represents the list of texts that you want to tokenize. You can pass a single string or a list of strings to be tokenized.
- **truncation**: By default, `truncation` is set to `False`. When set to `True`, it enables truncation of the input text if it exceeds the maximum sequence length specified by the tokenizer. Truncation can be useful when dealing with long texts that need to fit within a certain length constraint.
- **padding**: Similarly, the `padding` parameter is set to `False` by default. When set to `True`, it adds padding tokens to ensure all the tokenized sequences have the same length. Padding is commonly used when training or processing batches of sequences, as it allows for efficient batch processing.

By tokenizing the text, the tokenizer converts the input into a sequence of tokens, which are usually numerical or subword representations. The tokenizer can handle additional parameters as well, such as `max_length` to limit the length of the tokenized sequences or `return_attention_mask` to obtain the attention mask for the tokens.


# Example 3 - Hello Cat


```python
from transformers import AutoModelForCausalLM, AutoTokenizer
# Load the model and tokenizer
model_name = "meta-llama/Llama-2-7b-chat-hf"
model = AutoModelForCausalLM.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)
attention_mask_texts = ["The cat is black and white.", "The cat is very curious."]
prompt = "Write a story about a cat who goes on an adventure"
# Set the pad_token attribute of the tokenizer
tokenizer.pad_token = tokenizer.eos_token
# Create a batch of attention_mask_texts and the prompt
inputs = tokenizer(attention_mask_texts + [prompt], return_tensors="pt", padding=True)
# Get the input_ids and attention_mask tensors
input_ids = inputs["input_ids"]
attention_mask = inputs["attention_mask"]

# Generate text using the `model.generate()` function, passing in the prompt question and the attention mask tensor.
generated_text = model.generate(
    input_ids=input_ids,
    attention_mask=attention_mask,
    max_length=25,
)
```


```python
# Decode and print the generated text
generated_text = tokenizer.decode(generated_text[0], 
                                  skip_special_tokens=True)
print(generated_texto)
```

Notes . The type of  input_ids and attention_mask are torch.Tensor this is important at the moment to generate the text


```python
type(input_ids) , type(attention_mask)
```


    (torch.Tensor, torch.Tensor)


```python
def count_tokens(tensor):
    return tensor.size(-1)
```


```python
num_tokens = count_tokens(input_ids)
print("Number of tokens:", num_tokens)
```

    Number of tokens: 13

```python
num_tokens = count_tokens(attention_mask)
print("Number of tokens:", num_tokens)
```

    Number of tokens: 13

 This code is licensed under the MIT License.  ruslanmv.com 

**Congratulations!** We have learned how to generate text from a pretrained llama model.
