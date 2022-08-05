This is BERT Classifier model that we use.
It's in our code, however we upload separately because it's hard to understand.
```
def label(data_base):
    data_base['label']=np.nan
    predicted_label_list = []
    for text in data_base['desc']:
        # predict
        preds_list = text_classifier(text)

        sorted_preds_list = sorted(preds_list, key=lambda x: x['score'], reverse=True)
        predicted_label_list.append(sorted_preds_list[0]['label']) # label

    data_base['label'] = predicted_label_list
    return data_base.to_csv('sapago_live_data.csv')

MODEL_SAVE_PATH = 'data_classification_model/fine-tuned-kykim-bert-base'
loaded_tokenizer = BertTokenizer.from_pretrained(MODEL_SAVE_PATH)
loaded_model = TFBertForSequenceClassification.from_pretrained(MODEL_SAVE_PATH, id2label={0: 0 , 1: 1, 2: 2, 3: 3, 4: 4, 5: 5})

text_classifier = TextClassificationPipeline(
    tokenizer=loaded_tokenizer, 
    model=loaded_model, 
    framework='tf',
    return_all_scores=True
)

```