# re-study
The contents of this repository belong to a comparative study that aims to create an expressive baseline for comparing relation extraction approaches.

**FOR AUTHORS OF APPROACHES THAT WE SHOULD CONSIDER IN OUR STUDY: Please make sure that you carefully follow the instructions of the input data format as well as the description of the expected output format.**

# Descripton of provided data input format

Each dataset consists of three files: 

- `train.json` contains samples you can use for training your model
- `valid.json` contains samples for validation and model selection purposes
- `test.json` contains unseen samples that you will not see, but will be used by us to evaluate your approach

All files are provided in the same format. They consist of one JSON-object per line ([jsonl-format](https://jsonlines.org/)).
Each object has the following attributes:

- `id`: The id of a sample, used to identify your prediction later on
- `text`: The raw text of this sample. Note that this can be the white-space joined list of tokens, if the original dataset only provided pre-tokenized data
- `tokens`: A list of objects containing the following attributes
  - `text`: text of this token
  - `start`: start char index of this token (inclusive)
  - `stop`: stop char index of this token (exclusive)
  - `stanza_pos`: Part of speech tags (fine), obtained via "Stanza" (Stanford)
  - `stanza_dependency`: Dependency information obtained via "Stanza", consists of
    - `head`: 0-based index in the list of tokens for the token making up the dependency head
	- `type`: Dependency relation identifier as string
- `entities`: List of entities in this sample, comprised of objects with the following attributes:
  - `text`: Text of this entity
  - `token_indices`: 0-based indices of tokens that make up the given entity
- `relations`: List of relations found in this sample, given by objects with the following attributes:
  - `head_entity`: Entity that functions as source of this relation, object in the same form as objects in the list of `entities`
  - `tail_entity`: Entity that functions as target of this relation, object in the same form as objects in the list of `entities`
  - `type`: Relation type as string

An example that follows this format is [available](sample.jsonl).

# Description of expected output format

Given a test dataset, the output of your approach should be a file containing one line per sample in the given test set. 
Each line should be a JSON-object with the following attributes:

- `id`: Id of the sample this prediction was made for
- `relations`: A list of predicted relations, with one object per relation consisting of:
  - `head_entity`: an object describing the source entity by the following attributes    
    - `start`: start char index in the original text for this entity (inclusive). If your approach predicts non-continuous spans use a list for `start` and `stop`.
	- `stop`: stop char index in the original text for this entity (exclusive). If your approach predicts non-continuous spans use a list for `start` and `stop`.
  - `tail_entity`: an object describing the target entity of this relation, same format as `head_entity`
  - `type`: the predicted type of this relation as string
