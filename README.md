## Collab Emotion Mining Toolkit

### Table of Contents

[About](#about)

[Download](#download)

[How to run](#how-to-run)

[Programming languages, 3rd party libs, and OS](#programming-languages-3rd-party-libs-and-os)

[How to cite](#how-to-cite)

[License](#license)

### About EMTk

The emotion-mining toolkit comprises two main modules:
* [Emotion mining module](https://github.com/collab-uniba/Emotion_and_Polarity_SO) (formerly EmoTXT) - A ***general-purpose toolkit*** for training custom emotion classifiers from text. Together with the toolkit, we distribute an ***emotion classifier*** specifically tuned for emotion mining from  developers' communication channels, tranied using our gold standard of about [5k posts from Stack Overflow](https://github.com/collab-uniba/EmotionDatasetMSR18) 
* [Polarity mining module](https://github.com/collab-uniba/Senti4SD) (formerly Senti4SD) - An ***emotion-polarity*** classifier specifically trained on technical corpora from developers' communication channels

Choose **Collab EMTk** if:
* You need to assess the polarity of technical text snippets (e.g., issue comments) from the software development domain but you don't want to train your own classification model => select the [sentiment polarity module](https://github.com/collab-uniba/Senti4SD)
* You need to classify the emotion expressed in technical text snippets (e.g., commit comments) from the software development domain but you don't want to train your own classification model => use the classification function of the [emotion mining module](https://github.com/collab-uniba/Emotion_and_Polarity_SO).
* You have a corpus of text from any domain that you intend to use for training your own emotion classifier => use the training function of of the [emotion mining module](https://github.com/collab-uniba/Emotion_and_Polarity_SO).

### Download

#### The Docker image

To ease the installation and setup costs, we have packaged EMTk as a lightweight Docker container and published it on Docker-Hub at [this repository](https://hub.docker.com/r/collabuniba/emtk). Anyone interested in using EMTk must first install Docker. Then, to install the latest version of the EMTk container image, run the following from the command line:

```bash
$ docker pull collabuniba/emtk
$ docker run --rm -v <sharedFolderPath>:/shared -ti collabuniba/emtk
```

where `<sharedFolderPath>` is the path to the folder on the host that will be shared with the container to allow file exchange at runtime. 

#### Direct download

**EMTk** and all other software developed by Collab is available on [GitHub](https://github.com/collab-uniba). If you don't want to run the software from the Docker container, feel free to download directly the modules from their repos by clicking on any of the buttons below.

<a href="https://github.com/collab-uniba/Emotion_and_Polarity_SO">![EmoTXT](./img/button_emotion-module.png)</a>
&nbsp;
<a href="https://github.com/collab-uniba/pySenti4SD">![Senti4SD](./img/button_polarity-module.png)</a>

### How to run

First, execute the Docker container in interactive mode. By default, the instruction below will execute the `latest` version.

```bash
# docker run --rm -it collabuniba/emtk
```

##### Polarity module

The `-it` option starts the container in the interactive mode, so the `run` command logs you in the container’s shell environment (`>`). From there, to execute the polarity module, run:

```bash
> emtk polarity -F A -i input.csv -oc output.csv -vd 600 [-W dsm.bin] [-L] [-ul unigramList -bl bigramList]
```

where:

- `-F {A, S, L, K}`, feature to evaluate `A` for All, S for Semantic, `L` for Lexicon, `K` for Keyword.
- `-i <input.csv>`: the input data to classify. 
- `-oc <output.csv>`: the resulting predictions. 
- `-vd <N>`: the vector size.
- `-W <dsm.bin>`: [optional] the wordspace to use; the default wordspace is `/polarity/ClassificationTask/dsm.bin`.
- `-L`: [optional] if present, the input corpus comes with a gold label in the label column.
- `-ul <filename>`: [optional] the unigram's list.
- `-bl <filename>`: [optional] the bigram's list.

Users can test-drive the polarity module by using the file `/polarity_sample.csv`, containing only a handful of documents.

##### Emotion module

Regarding the emotion classifier module, in the following, we show first how to train a new model and, then, how to test it on unseen data. To train a new model on a training set, run:

```bash
> emtk emotions train -i file.csv -d delimiter [-g] -e emotion
```

where:

- `-i <file.csv>`: the corpus to be classified, encoded in UTF-8 without BOM and with the following format:

- ```
  id;label;text
  …
  22;NO;"""Excellent! This is exactly what I needed. Thanks!"""
  23;YES;"""FEAR!!!!!!!!!!!"""
  …
  ```

- `-d {c, sc}`: the delimiter used in the csv file, where c stands for comma and sc for semicolon.

- `-e {joy, anger, sadness, love, surprise, fear}`: the emotion to be detected.

- `-p`: enables the extraction of features regarding politeness, mood and modality.

As a result, the script will generate an output folder in the present working directory named `training_<file.csv>_<emotion>/`, containing:

- `n-grams/`: a subfolder containing the extracted n-grams.
- `idfs/`: a subfolder containing the IDFs computed for n-grams and WordNet Affect emotion words.
- `feature-<emotion>.csv`: a file with the features extracted from the input corpus and used for training the model.
- `liblinear/DownSampling/` and `liblinear/NoDownSampling/`, two folders each containing:
  - `trainingSet.csv` and `testSet.csv`.
  - eight models trained with `liblinear model_<emotion>_<ID>.Rda`, where ID refers to the liblinear model (with values in {0, ..., 7}).
  - `performance_<emotion>_<IDMODEL>.txt`, a file containing the results of the parameter tuning for the model (cost), the confusion matrix, and the Precision, Recall, and F-measure for the best cost for the specific `<emotion>`.
  - `predictions_<emotion>_<IDMODEL>.csv`, containing the test instances with the predicted labels for the specific `<emotion>`.

Finally, to execute the classification task, run:

```bash
> emtk emotions classify -i file.csv -d delimiter -e emotion [-m model] [-f /path/to/.../idfs] [-o /path/to/.../ngrams] [-l]
```

where:

- `-i <file.csv>`: same as above.
- `-p`: enables the extraction of features regarding politeness, mood and modality.
- `-d {c, sc}`: same as above.
- `-e {joy, anger, sadness, love, surprise, fear}`: same as above.
- `-m model`: [optional] the model file learned during the training step; if not specified, as default the model learned on the Stack Overflow gold standard will be used.
- `-f /path/to/.../idfs`: [optional] with custom models, also the path to the folder containing the dictionaries with IDFs computed during the training step is required; the folder must include IDFs for n-grams (uni- and bi-grams) and the WordNet Affect lists of emotion words.
- `-o /path/to/.../ngrams`: [optional] with custom models, also the path to the folder containing the dictionaries extracted during the training step; the folder must include n-grams (i.e., UnigramsList.txt and BigramsList.txt).
- `-l`: [optional] if present, the input corpus comes with a gold label in the column label.

As a result, the script will create an output folder in the present working directory named `classification_<file.csv>_<emotion>`, containing:

- `predictions_<emotion>.csv`: a csv file, containing a binary prediction (yes/no) for each line of the input corpus:

- ```
  id;predicted
  …
  22;NO
  23;YES
  …
  ```

- `performance_<emotion>.txt`: a file containing several performance metrics (Precision, Recall, F1, confusion matrix), created only if the input corpus `<file.csv>` contains the column label.

Users can test-drive the emotion classification module by using the file `/emotions_sample.csv`, which contains only a handful of documents. Other more complex sample datasets are available at `/emotions/java/DatasetSO/StackOverflowCSV`.

##### The `/shared/` folder

To use the EMTk modules with **custom datasets**, users must access the `/shared/` folder, which is mounted specifying the `–v`  option in the `docker run` command shown above. The `-v` option defines the paths for the folder to be shared in both the host and the hosted machines:

`docker run -v <pathInTheHostMachine>:<pathInTheContainer> [...]`.

For instance, on a Linux machine, `-v ~/shared:/shared` creates a folder named `shared` in the host system's home (if it doesn't already exist) and a folder named `shared` in the container's root. Whatever is put into the shared folder can be found on both the systems, allowing input and output file exchange. This is accomplished by leveraging [Docker's bind mounts](https://docs.docker.com/storage/bind-mounts/).

### Programming languages, 3rd party libs, and OS

**Collab EMTk** is developed using a mix of Java, Python, R. Hence, it works on Linux, macOS, and Windows. The following 3rd party libraries are also used:
* [NLTK](http://www.nltk.org)
* [Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/index.html)
* [SentiStrength](http://sentistrength.wlv.ac.uk)

### How to cite

If you intend to use the **Collab EMTk** for your work, please cite the following papers:

* F. Calefato, F. Lanubile, and N. Novielli (2017) <a href="https://arxiv.org/pdf/1709.02984.pdf">"Sentiment Polarity Detection for Software Development."</a> *Empirical Software Engineering Journal*, DOI: <a href="http://dx.doi.org/10.1007/s10664-017-9546-9">10.1007/s10664-017-9546-9</a>. 
```latex
@article{calefato2017emse,
 author="Calefato, Fabio and Lanubile, Filippo and Maiorano, Federico and Novielli, Nicole",
 title="Sentiment Polarity Detection for Software Development",
 journal="Empirical Software Engineering",
 year="2017",
 issn="1573-7616",
 doi="10.1007/s10664-017-9546-9",
 url="https://doi.org/10.1007/s10664-017-9546-9"
}
```

* F. Calefato, F. Lanubile, N. Novielli (2017) <a href="https://arxiv.org/pdf/1708.03892.pdf">"EmoTxt: A Toolkit for Emotion Recognition from Text."</a> In *Proc. 7th Affective Computing and Intelligent Interaction (ACII’17)*, San Antonio, TX, USA, Oct. 23-26, 2017.
```latex
@inproceedings{calefato2017acii,
 title={EmoTxt: A Toolkit for Emotion Recognition from Text},
 author={Calefato, Fabio and Lanubile, Filippo and Novielli, Nicole},
 booktitle = {Proc. of 7th Int'l Conf. on Affective Computing and Intelligent Interaction Workshops and Demos},
 series = {ACII 2017},
 year = {2017},
 isbn = {978-1-5386-0563-9},
 doi = {10.1109/ACIIW.2017.8272591},
 url ={http://doi.ieeecomputersociety.org/10.1109/ACIIW.2017.8272591},
 location = {San Antonio, TX, USA},
 pages = {79--80},
 numpages = {2}
}
```
* N. Novielli, F. Calefato, F. Lanubile (2018) <a href="https://arxiv.org/pdf/1803.02300.pdf">"A Gold Standard for Emotions Annotation in Stack Overflow."</a>  In *Proc. of the 15th International Conference on Mining Software Repositories* (MSR 2018), Gothenburg, Sweden, May 28-29, 2018.
```latex
@inproceedings{Novielli2018msr,
 title={A Gold Standard for Emotions Annotation in Stack Overflow},
 author={Novielli, Nicole and Calefato, Fabio and Lanubile, Filippo},
 booktitle = {Proc. of 15th Int'l Conf. on Mining Software Repositories},
 series = {MSR 2018},
 year = {2018},
 isbn = {978-1-4503-5716-6/18/05},
 doi = {10.1145/3196398.3196453},
 location = {Gothenburg, Sweden},
 pages = {14--17},
 numpages = {4}
}
```

### License

**Collab EMTk** is licensed under the [MIT License](https://github.com/collab-uniba/emtk/blob/master/LICENSE).

### Support or Contact

Having trouble with our toolkit? [Contact us](http://collab.di.uniba.it/members) and we’ll help you sort it out.
