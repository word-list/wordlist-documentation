# Queues and Messages

## update-from-source-queue

### Purpose
Messages for the `update-from-source` lambda which trigger an update from a word source.

### Message Format
```json
{
    "name": "name-of-source",
    "force": false // true to update all words even if they already exist
}
```

## query-word-queue

### Purpose
Messages for the `query-word` lambda which triggers a (batched) async LLM query for one or more words.

### Message Format
```json
{
    "word": "word-to-query",
    "force": false // true to update the word even if it exists
}
```

## update-batch-queue

### Purpose
Messages for the `update-batch` lambda which retrieves the status of a batched LLM task:
- If the batch is completed, words are posted to the `update-word-queue` below
    - Any missing words are reposted to the `query-word-queue`.
- If the batch failed, the failure is recorded.

### Message Format
```json
{
    "batchId": "batchId-to-update"
}
```

## update-word-queue

### Purpose
Messages for the `update-word` lambda which updates stored words.

### Message Format
```json
{
    "word": "word-to-update",
    "offensiveness": 0, // 0 to 5 where 5 is extremely offensive
    "commonness": 0,    // 0 to 5 where 5 is extremely common
    "sentiment": 0,     // -5 to 5 where -5 is extremely negative and +5 is extremely positive
    "types": [ "noun" ] // types of word
}
```
