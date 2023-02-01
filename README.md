# Cantonese Word Segmentation Benchmark

`benchmark.ipynb` uses HkCanCor utterances as ground truth to benchmark the word segmentor included in the pycantonese package. We measure three things:

1. Accuracy = (# of correctly segmented utterances) / (# of utterances)
2. Macro recall = (# of correct segments) / (# of segments in ground truth)
3. Micro recall = avg( (# of correct segments in an utterance) / (# of segments in an utterance according to the ground truth) )

## Results
1. Accuracy: 60.2%
2. Macro recall: 0.893
3. Micro recall: 0.871

After inspecting the incorrectly segmented utterances, we found that many of them are due to different granularity between the pycantonese segmentor and the ground truth segmentations of HkCanCor. Often, HkCanCor prefers to break up phrases like "遲啲" into "遲" and "啲", "係唔係" into "係" and "唔係". Pycantonese also concatenates all neighboring English phrases together which HkCanCor keeps them seperate. There is a notable actual segmentation issue by pycantonese:

* HkCanCor segmentation: 平 機票 要 淡季 **先 有得** 平 𡃉 喎 .
* pycantonese segmentation: 平 機票 要 淡季 **先有 得** 平 𡃉 喎 .

Because pycantonese uses "a simple longest string matching algorithm", it recognizes "先有" as a word followed by the word "得". However in reality, the correct segmentation should be "先" followed the word "有得". Those kind of actual segmentation issues are extremely rare so we observe that for conversational Cantonese, using a longest string matching algorithm might be good enough for most cases.

## Limitations

Because the pycantonese segmentor is trained on HkCanCor data, it have seen all the words in the corpus. Thus, the performance of the segmentor measured against HkCanCor utterances might be higher than actual performance on unseen utterances.
