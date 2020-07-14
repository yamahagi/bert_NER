# bert_NER

入力データ例(NLI)
label(true or false)+"\t"+PDF Id + "\t" + text_a(premise) + "\t" + text_b(hypothesis) + "\t" + NER ids for each subtoken(0,1,3,5のどれかの数字の列）

BERTのrepresentationにNERのembeddingsを加えるためのコード

基本的にはscience-result-extractor+BERTのコードと同じ(https://github.com/IBM/science-result-extractor) なのですが、一部違うところがあるのでそこだけ書いておきます。


run_classifier_sci.py内
InputExample(): NERデータを保持するように変更 \n
InputFeatures(): NERデータを保持するように変更
class SciProcessor(DataProcessor): 元データからNERデータも読み出してInputExampleのリストを作るように変更
convert_single_example(): (ここでInputExampleをInputFeaturesに変換)NERデータを含めた変更
file_based_convert_examples_to_features(): NERデータを受け入れるように変更
file_based_input_fn_builder(): NERデータを受け入れるように変更
create_model(): NERデータを受け入れるように変更
model_fn_builder(): def model_fn()内でfeaturesからner idを読み込んでモデルに入力するように変更
model_fn_builder()でInput exampleをcreate_model()を通じてBERTに入力。

modeling.py内
BertModel(object): NERデータを受け入れるように変更+embedding_postprocessor()部分でNERデータのrepresentationを加えられるように変更
embedding_postprocessor(): if use_ner:の部分でner embeddingsを組み合わせるように変更(vocabulary sizeは4に設定)
