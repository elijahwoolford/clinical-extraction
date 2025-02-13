This notebook takes the given clinical notes, extracts medical info into fields that I deem as useful and then evaluates the extractions. Below is a description of the files you'll find in this project and the process I used to create and verify these files.

- **Running**
  - If you'd like to run the notebook you can create a .env file and set API_KEY=XXXX to your openAPI key. For this exercise I utilized my own personal key.

- **Output Data**
  - clinical_data.csv: Downloaded raw data of clinical notes
  - error.csv: Contains fields that the evaluation gave a score of less than 1.0
  - extractions.json: Raw extractions from LLM without evaluation changes
  - cleaned_extraction.json: Cleaned extraction based off of evaluation method. Replaces unsure fields with unknown
  
- **LLM-Based Approach**: Chose an LLM for extraction and evaluation due to its superior accuracy and flexibility compared to NER or regex methods. I utilized langchain with prompt templates for efficient prompt creation.

- **Extraction Method**: Used a mass prompt and prompt template for extraction, including the clinical note, field specifications, and return types to ensure consistency. 
- **JSON Verification**: Before storing the extractions, I verify that first a json is returned from the LLM and then verify the fields within the json and replace any missing fields with unknown
- **Selected Fields**: These fields were chosen based on their clinical relevance and utility for doctors. Extracted key fields:  
  - `patient_name`  
  - `age`  
  - `gender`  
  - `medical_record_number`  
  - `symptoms`  
  - `diagnoses`  
  - `medications`  
  - `lab_results`  
  - `next_steps`  
- **Evaluation Method**: Employed a per-field evaluation prompt, providing the field name, extracted value, and clinical note for the LLM to verify accuracy. I utilized a quantitative and qualitative approach in evaluation. The eval will return a score of 0-1 with 1 being a perfect extraction as well as an explanation of why the score was given.  
- **Hallucination Handling**: Removed explicit hallucinated fields with during evaluation and replaced them with unknown. An example of that is Thomas Wilson in the dataset. He has anxiety about the buffet options that was extracted as part of his symptoms but it isn't relevant. I decided to replace symptoms as unknown. Depending of the business need this maybe sufficient or with more time we'd want to analyze these extractions further.
- **Limitations**:  
  - Scalability: Per-field evaluation can become computationally expensive with large datasets.  
  - Token Limits: Large notes may exceed API token limits, requiring truncation or chunking. 
  - Cost: Doing a LLM approach for both extraction and evaluation can be costly at scale

- **Potential Next Steps**: With only 2 hours to conduct this pipeline e2e, there are some of ideas that come to mind I would like to try if I had more time. 
  - Other LLMs for evaluation(gpt4o-large and deepseek)
  - Establish a manual ground truth for analysis to improve evaluation
  - Combo approaches with NER to verify simple extracted entities.
  - Remove well extracted fields from eval prompt if it's doing really well to improve evaluation efficiency.

- **Total time taken**: 2 Hours 11 minutes
