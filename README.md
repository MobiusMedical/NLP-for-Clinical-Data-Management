# NLP-for-Clinical-Data-Management
## Brief Project Description

This project contains a series of Python scripts / Jupyter Notebooks with Natural Language Processing (NLP) capabilities that can be used to support traditional Clinical Data Management activities. The project currently consists of a MedDRA autocoder tool and a series of programatic data cleaning checks that would otherwise typically involve manual review of free-text information.

The python scripts are experimental in nature and are intended mainly to provide examples / inspiration on how simple NLP can be implemented in everyday Clinical Data Management tasks to support and backup manual processes. Until you are familiar with the behaviour and performance, it is recommended that the outputs always be validated and reviewed before coding to any dictionary terms or raising any queries. It may be possible to convert these scripts into SAS equivalents - If so, please share the SAS code back into this repository if possible.

Additional AI-related clinical data management checks and tools may be added to this project as it evolves.

## Authors & Contributors

Initial Author:
* Thomas Choat

Contributors:

## Software Requirements

The following software versions or higher are required for this project. Earlier versions will probably work but have not been tested:

* Python 3.11.1
* Jupyter Notebook 6.5.2

## Dependencies

The following Python Library versions or higher are required for this project. Earlier versions will probably work but have not been tested:

* Pandas 2.0.0
* Numpy 1.24.2
* OpenAI 0.27.4

## Detailed Project Information

The Python scripts in this repository communicate with OpenAI's GPT-3 language models via an encrypted API connection. Please see OpenAI's 'API data usage' and 'API data privacy' policies to evaluate compatability with your organisation's data handling requirements. Specifically, the 'text-embedding-ada-002' model is used to represent natural language as numerical vectors, on which NLP tasks can then be performed. The scripts may be updated to work with newer embedding models as they become available, or you may wish to adapt them for use with a locally hosted LLM to avoid third party data handling.

### MedDRA Autocoder
This program takes your raw EDC-exported AE dataset and compares it with the MedDRA dictionary of available LLTs. OpenAI's 'text-embedding-ada-002' model is then accessed by the program to get numerical representations of all AE terms and LLTs, which are then semantically compared, and the X highest similarity matches (as specified by you) are provided in the output. The program does not identify situations where the AE term first needs querying (e.g. if further qualifying information is required in the term) - It will attempt to find the highest semantic LLT match based on the AE term that is present.

### MH vs AE Similarity Check
This check takes your raw EDC-exported MH and AE datasets and compares the MH and AE terms to identify any that are semantically very similar or the same within each subject (considers ongoing MH only). The purpose of this check is to flag AEs that might really be considered part of the subject's baseline ongoing medical history that may need to be removed or prefixed with 'worsening' or 'exacerbation'.

### Reconciliation Checks Between AE, CM, MH, PR
These are a suite of similar checks that perform equivalent reconciliation tasks between these datasets. For example, the 'AE vs CM Reconciliation' check takes your raw EDC-exported AE and CM datasets and reconciles them. It compares the AE terms and dates with CM indications and dates within each subject to identify any AEs where other action taken is 'Medication' but a medication with matching indication and/or senseful dates cannot be found. The program assumes your AE dataset has an 'AEACNOTH_DRUG' field to indicate if a medication was taken for the AE. It also assumes your CM dataset has a 'CMINDC_CAT' field to indicate if the medication was taken for an AE, however it can be updated if your CM datset does not have this field. If a sufficiently similar AE term / CM indication match is found, the program then compares the timeframe of the AE with the timeframe of the CM, to see if the medication was taken within the timeframe of the AE. Flagged issues will fall into two categories: 1) "Matching indication found in CM, but dates inconsistent", or 2) "Matching indication not found in CM". For reconciliation checks involving MH and CM, the program handles partially unknown dates such as UNK-01-2023 and UNK-UNK-2023, and in such cases it will only flag issues if it is NOT POSSIBLE for the dates to be sensefully related. All programs in this suite of checks function in a very similar way but are uniquely tailored to the various nuances of the specific datasets involved.

### Duplicate & Overlapping AE, CM, MH Checks
These are a suite of similar checks that perform equivalent tasks for each of these datasets. For example, the 'Duplicate & Overlapping AE Check' takes your raw EDC-exported AE dataset and compares it with itself. It does pairwise comparisons of all AEs within each subject to identify any that have semantically similar AE terms and overlapping timeframes. The purpose of this check is to help identify similar overlapping AEs that could be duplicate entries or otherwise considered for consolidation into a single AE. It considers dates only, so similar AEs that occur on the same day but at different times will flag on the output, however the start and end times will be shown on the output for your reference. For checks involving MH and CM, the program handles partially unknown dates such as UNK-01-2023 and UNK-UNK-2023, and in such cases it will only flag issues if it IS POSSIBLE for the dates to overlap. All programs in this suite of checks function in a very similar way but are uniquely tailored to the various nuances of the specific datasets involved.

## Downloading the Project Contents

If you would like a copy of the Python scripts / Jupyter Notebooks for your own use, simply click the green 'Code' button on the main repository page and then click 'Download ZIP'.

## Contributing to the Project

Contributions to this project are welcome!

If you have any ideas for improving the exisitng Python scripts / Jupyter Notebooks or would like to contribute a completely new tool or feature to the project, please start a discussion in the 'Discussions' section.

After discussing, you can contribute to the project by doing the following:

1. Fork the main repository and then clone it to your local machine.
2. Create a new branch in your local repository to work on the changes.
3. Make the changes and then commit them to the branch you are working in.
4. Push the changes to your forked repository.
5. Initiate a pull request for the changes in your forked repository to be merged back into the main upstream repository.

## License

This project is licensed under the MIT License.