|  | **`IKTokenizer`** | **`StanfordTokenizer`** |
|:-|:------------------|:------------------------|
| **官方网址** | <a href='http://code.google.com/p/ik-analyzer/'>IKAnalyzer</a> | <a href='http://nlp.stanford.edu/software/segmenter.shtml'>Stanford Word Segmenter</a> |
| **相关jar文件** | flashdb-ik-4.7.2.jar | stanford-segmenter-2013.04.04.jar,flashdb-extention-1.0.1.jar |
| **词典数据文件夹** | ik-dict | stanford-segmenter-data |
| **分词技术** | 基于词典库的机械分词 | 基于CRF的统计分词 |
| **优点** | 速度快，内存占用小。词典文件修改简单 |  对新词汇的识别较准  |
| **缺点** | 新词汇必须人工维护到词典中 |  内存占用较大(1G). 中英文混合的处理不好 |
| **适用场景** | 一般场景 | 中文为主的新闻类文章搜索 |

<br />
备注: Stanford Word Segmenter的词典数据文件较大，需自行到官方网站上下载解压。将其中的data目录复制为 ${FLASHDB\_HOME}/plugin/stanford-segmenter-data 目录.