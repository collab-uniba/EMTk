## Collab Emotion Mining Toolkit

### Table of Contents

[About](#about)

[Download](#download)

[Programming languages, 3rd party libs, and OS](#programming-languages-3rd-party-libs-and-os)

[License](#license)

[How to cite](#how-to-cite)

### About EMTk

The emotion-mining toolkit comprises the following software:
* [EmoTXT](https://github.com/collab-uniba/Emotion_and_Polarity_SO) - A ***general-purpose toolkit*** for training custom emotion classifiers from text. Together with the toolkit, we distribute an ***emotion classifier*** specifically tuned for emotion mining from  developers' communication channels, tranied using our gold standard of about [5k posts from Stack Overflow](https://github.com/collab-uniba/EmotionDatasetMSR18) 
* [Senti4SD](https://github.com/collab-uniba/Senti4SD) - An ***emotion-polarity*** classifier specifically trained on technical corpora from developers' communication channels

Choose **Collab EMTk** if:
* You need to assess the polarity of technical text snippets (e.g., issue comments) from the software development domain but you don't want to train your own classification model => select [Senti4SD](https://github.com/collab-uniba/Senti4SD)
* You need to classify the emotion expressed in technical text snippets (e.g., commit comments) from the software development domain but you don't want to train your own classification model => use the classification function of [EmoTXT](https://github.com/collab-uniba/Emotion_and_Polarity_SO).
* You have a corpus of text from any domain that you intend to use for training your own emotion classifier => use the training function of [EmoTXT](https://github.com/collab-uniba/Emotion_and_Polarity_SO)

### Download

**EMTk** and all other software developed by Collab is available on [GitHub](https://github.com/collab-uniba). If you don't want to clone the repos, click on any of the buttons below to download directly.

<a href="https://github.com/collab-uniba/Emotion_and_Polarity_SO/archive/master.zip">![EmoTXT](./img/button_emotxt.png)</a>
&nbsp;
<a href="https://github.com/collab-uniba/Senti4SD/archive/master.zip">![Senti4SD](./img/button_sentisd.png)</a>

### Programming languages, 3rd party libs, and OS

**Collab EMTk** is developed using a mix of Java, Python, R. Hence, it works on Linux, macOS, and Windows. The following 3rd party libraries are also used:
* [NLTK](http://www.nltk.org)
* [Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/index.html)
* [SentiStrength](http://sentistrength.wlv.ac.uk)

### License

**Collab EMTk** is licensed under the [MIT License](https://github.com/collab-uniba/emtk/blob/master/LICENSE).

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
 booktitle = {Proceedings of the Seventh International Conference on Affective Computing and Intelligent Interaction Workshops and Demos},
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
* N. Novielli, F. Calefato, F. Lanubile (2018) <a href="">"A Gold Standard for Emotions Annotation in Stack Overflow."</a> To appear in *Proc. of the 15th International Conference on Mining Software Repositories* (MSR 2018), Gothenburg, Sweden, May 28-29, 2018.
```latex
@inproceedings{Novielli2018msr,
 title={A Gold Standard for Emotions Annotation in Stack Overflow},
 author={Novielli, Nicole and Calefato, Fabio and Lanubile, Filippo},
 booktitle = {Proc. of the 15th International Conference on Mining Software Repositories},
 series = {MSR 2018},
 year = {2018},
 isbn = {??},
 doi = {??},
 location = {Gothenburg, Sweden},
 pages = {??--??},
 numpages = {4}
}
```

### Support or Contact

Having trouble with our toolkit? [Contact us](http://collab.di.uniba.it/members) and we’ll help you sort it out.
