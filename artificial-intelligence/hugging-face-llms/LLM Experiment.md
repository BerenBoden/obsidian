!pip install transformers einops accelerate bitsandbytesfrom transformers import AutoTokenizer, AutoModelForCausalLM
from transformers import pipeline
import torch
import base64checkpoint = "ehartford/WizardLM-7B-Uncensored"!pip install safetensorstokenizer = AutoTokenizer.from_pretrained(checkpoint)
base_model = AutoModelForCausalLM.from_pretrained(checkpoint)!pip install langchainfrom langchain.llms import HuggingFacePipelinedef llm_pipeline():
    pipe = pipeline(
        'text2text-generation',
        model = base_model,
        tokenizer = tokenizer,
        max_length = 256,
        do_sample=True,
        temperature = 0.3,
        top_p = 0.95
    )
    local_llm = HuggingFacePipeline(pipeline=pipe)
    return local_llminput_prompt = "prompt"model = llm_pipeline()
generated_text = model(input_prompt)
generated_textinput_prompt = "prompt1"
generated_text = model(input_prompt)
generated_textinput_prompt = "prompt2"
generated_text = model(input_prompt)
generated_text!pip install transformers einops accelerate bitsandbytesfrom transformers import AutoTokenizer, AutoModelForCausalLM
from transformers import pipeline
import torch
import base64checkpoint = "ehartford/WizardLM-7B-Uncensored"!pip install safetensorstokenizer = AutoTokenizer.from_pretrained(checkpoint)
base_model = AutoModelForCausalLM.from_pretrained(checkpoint)!pip install langchainfrom langchain.llms import HuggingFacePipelinedef llm_pipeline():
    pipe = pipeline(
        'text2text-generation',
        model = base_model,
        tokenizer = tokenizer,
        max_length = 256,
        do_sample=True,
        temperature = 0.3,
        top_p = 0.95
    )
    local_llm = HuggingFacePipeline(pipeline=pipe)
    return local_llminput_prompt = "prompt3"model = llm_pipeline()
generated_text = model(input_prompt)
generated_textinput_prompt = "prompt4"
generated_text = model(input_prompt)
generated_textinput_prompt = "prompt5"
generated_text = model(input_prompt)
generated_text