## 文本摘要的评价指标-ROUGE
#### Recall-Oriented Understudy for Gisting Evaluation
### precision和recall的定义
用n表示重复词的个数，$n_y$表示参考摘要的长度，$n_{pred}$表示预测摘要的长度

$$precision=\frac{n}{n_{pred}}$$
$$recall=\frac{n}{n_y}$$
重叠词即标准摘要与预测摘要的重复词个数。recall是重叠词个数与标准摘要的词个数的比值。而precision是重叠词个数与预测摘要的词个数的比较。
### 例1：
* 参考摘要：<u>the</u> <u>cat</u> <u> was</u> <u>under</u> <u>the</u> <u>bed</u>
* 预测摘要：<u>the</u> <u>cat</u> <u> was</u> found <u>under</u> <u>the</u> <u>bed</u>

$$recall=\frac{6}{6}=1$$
$$precision=\frac{6}{7}$$

### ROUGE-1, ROUGE-2, ROUGE-L：
主要关乎是几元语法，rouge1代表一元，rouge2代表二元，rouge-L代表最长公共子串长度。以上述例1为例：
#### ROUGE-1:
$$Rouge1_{recall}=\frac{6}{6}=1$$
$$Rouge1_{precision}=\frac{6}{7}$$
$$Rouge1_{F}=\frac{2*precision*recall}{precision+recall}=\frac{12/7}{13/7}=\frac{12}{13}$$
#### ROUGE-2:
标准摘要分解成二元：

<u>(the cat)</u> <u>(cat was)</u> (was under) <u>(under the)</u> <u>(the bed)</u>

模型摘要:

<u>(the cat)</u> <u>(cat was)</u> (was found) (found under) <u>(under the)</u> <u>(the bed)</u>

$$Rouge2_{recall}=\frac{4}{5}$$
$$Rouge2_{precision}=\frac{4}{6}$$
$$Rouge2_F=\frac{2*\frac{4}{5}*\frac{4}{6}}{\frac{4}{5}+\frac{4}{6}}=\frac{8}{11}$$
#### ROUGE-L
此处的L表示最长公共子串LCS

标注摘要：

the cat was <u>under the bed</u>

参考摘要：

the cat was found <u>under the bed</u>

故LCS长度为3

$$RougeL_{recall}=\frac{3}{6}$$
$$RougeL_{precision}=\frac{3}{7}$$
$$RougeL_F=\frac{6}{13}$$
