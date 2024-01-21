# NLP-for-Clinical-Data-Management
## Brief Project Description

This project contains example Python scripts with Natural Language Processing (NLP) capabilities aimed at supporting Clinical Data Management activities. It consists of an eCRF Completion Guidelines Text Generator Tool (for Dacima EDC system), MedDRA coding tool, and some NLP data cleaning checks that would typically involve full manual review of text data. The scripts are experimental in nature and are only intended to provide examples of how NLP can be implemented in Clinical Data Management to aid manual tasks.

PLEASE NOTE: The Python scripts are connected to OpenAI models purely for ease of sharing and demonstrating the FUNCTION of the code and it does NOT suggest this is the recommended mode of implementation for your sepcific use case from a data privacy and security standpoint. Robust data privacy, security and ethical considerations should always be taken into account if and when using AI algorithms on patient data. The purpose of this repository is not to advise on this aspect, but rather to provide example code for interacting with LLMs and embedding models in general. Until you are familiar with the behaviour and performance, it is recommended that the outputs always be validated and reviewed before using.

Additional AI tools may be added to this project as it evolves.

## Authors & Contributors

Author:
* [Thomas Choat](https://github.com/tchoat)

## Software Requirements

The following software versions or higher are required for this project:

* Python 3.11.1
* Jupyter Notebook 6.5.2

## Dependencies

For the eCRF Completion Guidelines Text Generator tool, see the Python library versions in the 'requirements.txt' file in the 'eCRF Completion Guidelines Generator' folder.

For all other tools, the following library versions are recommended:

* Pandas 2.0.0
* Numpy 1.24.2
* OpenAI 0.27.4

## Detailed Project Information

The Python scripts in this project are connected to OpenAI's GPT-3 family of language models, namely 'text-davinci-003' and 'text-embedding-ada-002', purely for ease of sharing and demonstrating the FUNCTION of the code - It does NOT suggest this is the recommended mode of implementation for your sepcific use case from a data privacy and security standpoint. The scripts may be adjusted to work with different models, e.g. locally hosted LLM, or on a platform with additional enterprise security and compliance features such as Azure OpenAI Service. Robust data privacy, security and ethical considerations should always be taken into account if and when using AI algorithms on patient data. The purpose of this repository is not to advise on this aspect, but rather to provide example code for interacting with LLMs and embedding models in general.

### eCRF Completion Guidelines Text Generator
This tool is specifically designed for use with reports from the Dacima EDC system, however can likely be adapted to work with reports extracted from other EDC systems. It uses the LangChain Python library to expand information and logic stored in various EDC configuration reports (e.g. data dictionary, items, formulas, lists, dependencies, form activations, visit association reports etc.) into expressive natural language to be used in eCRF Completion Guidelines documents. It's an efficient way to establish a base-level of content in your guidelines document, which can then be built up or cut down where necessary.

### MedDRA Autocoder
This is an AI-assisted semantic search tool for the MedDRA dictionary. It takes your raw EDC-exported AE dataset and compares it with the MedDRA dictionary of available LLTs. The 'text-embedding-ada-002' model is accessed by the program to get numerical vector representations of all AE terms and LLTs, which are then semantically compared using cosine similarity, and the X highest similarity matches (as specified) are provided in the output.

### MH vs AE Similarity Check
This check takes your raw EDC-exported MH and AE datasets and compares the MH and AE terms to identify any that are semantically very similar or the same within each subject (considers ongoing MH only). The purpose of this check is to flag AEs that might really be considered part of the subject's baseline ongoing medical history that may need to be removed or clarified as 'worsening'.

### Reconciliation Checks Between AE, CM, MH, PR
These are a suite of similar checks that perform equivalent reconciliation tasks between these datasets. For example, the 'AE vs CM Reconciliation' check takes your raw EDC-exported AE and CM datasets and reconciles them. It compares the AE terms and dates with CM indications and dates within each subject to identify any AEs where other action taken is 'Medication' but a medication with matching indication and/or senseful dates cannot be found. The program assumes your AE dataset has an 'AEACNOTH_DRUG' field to indicate if a medication was taken for the AE. If a sufficiently similar match between AE term and CM indication is found, the program then compares the timeframe of the AE with the timeframe of the CM, to see if the medication was taken within the timeframe of the AE. Flagged issues will fall into two categories: 1) "Matching indication found in CM, but dates inconsistent", or 2) "Matching indication not found in CM". For reconciliation checks involving MH and CM, the program handles partially unknown dates such as UNK-01-2023 and UNK-UNK-2023, and in such cases it will only flag issues if it is NOT POSSIBLE for the dates to be sensefully related.

### Duplicate & Overlapping AE, CM, MH Checks
These are a suite of similar checks that perform equivalent tasks for each of these datasets. For example, the 'Duplicate & Overlapping AE Check' takes your raw EDC-exported AE dataset and compares it with itself. It does pairwise comparisons of all AEs within each subject to identify any that have semantically similar AE terms and overlapping timeframes. The purpose of this check is to help identify similar overlapping AEs that could be duplicate entries or otherwise considered for consolidation into a single AE. It considers dates only, so similar AEs that occur on the same day but at different times will flag on the output, however the start and end times will be shown on the output for your reference. For checks involving MH and CM, the program handles partially unknown dates such as UNK-01-2023 and UNK-UNK-2023, and in such cases it will only flag issues if it IS POSSIBLE for the dates to overlap.

## Downloading the Project

If you would like a copy of the Python scripts, simply click the green 'Code' button on the main repository page and then click 'Download ZIP'.

## Contributing to the Project

Contributions to this project are welcome!

If you have any ideas for improving the exisitng Python scripts or would like to contribute a completely new tool or feature, please start a discussion in the 'Discussions' section.

## License

This project is licensed under the MIT License.