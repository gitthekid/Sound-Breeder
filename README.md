Problem Statement:

Can I utilize a neural network architecture to combine two different sounds into a new composite sound? Furthermore, can I do this in a way that would be compositionally useful when making music?

This interest arises from the immense breakthroughs in visual GANs, in particular ArtBreeder, a conolutional neural network based model that allows for intelligent merging of different images.
https://www.artbreeder.com/



EDA:
There are countless audio generation strategies including WaveNet, GanSynth, and DDSP, amongst numerous others. I chose DDSP, an autorgressive model that uses synthesizers to generate audio signals by automating filters, oscillators, and reverbation to adapt the waveform realtime.
I chose DDSP because it:
    Has relatively the fastest training times
    Does not need to be trained on pre-tagged data, and thus has extremely broad creative applications
    The tone/timbre of each instrument can applied to any monophonic audio sample
    DDSP is highly transparent, and highly tweakable (pitch/loudness/reverb)
    
DDSP must be trained on monophonic, that is non-chordal data.
As such the data I brought in had to be cleaned to meet this criteria.

I modeled 6 sounds, John Lennon vocals, Paul McCartney vocals, a model trained on both Lennon and McCartney Vocals, Cello, Electric Guitar, and a model trained on both Cello and Electric Guitar.

I trained the Beatles vocals on isolated vocals on youtube, and chopped them up to make sure only monophonic information was in the training data use mp3cutter.net. They were trained on many separate songs vocals, meaning many different reverberations, and vocal tones. There was about 10 minutes of audio to train the Lennon model, and 10 minute of audio to train the McCartney model, and 20 minutes of audio to train their composite model.

I trained the Cello/Electric Guitar models on individual youtube videos of Cello Suite No.1 by Bach, that were both 20 minutes long. This data did not need to be cleaned, as Cello Suite No.1 is almost entirely monophonic. 


Modeling and Results:
The Neural Network used in DDSP is mostly imported with the DDSP library. However, to describe its architechture nonetheless:
It is a feed forward neural network with an architechture including two Encoders for pitch (via CREPE, a pre-trained model with fixed weights), and loudness, and one decoder comosed of 2 MLP layers going into a concatenation layer, a GRU layer, a concatenation layer, another MLP layer, into two desne layers that ouput into the additive and subtractive synthesizers.
It uses the Adam Optimization function, and a Multi-Scale Spectral Loss Function. Further it has 165,000 dimensions.


The notebooks are split up into a Train Autencoder Notebook, where we train the DDSP model to resynthesize the audio, and a Timbre Transfer Notebook, where we apply the trained autencoder to new monophonic pitch, and loudness instruments.

Judging the effectiveness of modelled audio is best done by listening to it, rather than through graphs. As such, all the generated audio, alongside their performance graphs, including loss functions are included in the Autencoder folder.



Conclusions:
The voice model had many weaknesses, and by and large did not sound accurate to the source sounds. 
This is likely due to many factors including:
    Overly complex audio information as speech/singing has too many tones to easily model
    Training audio was from different recordings with different room noise.
The cello/guitar model had more accurate tone modeling than the voices, particularly the cello. However, none of the models were accurate enough at modeling the source sound to be ideal.   


That being said:
In conclusion, I was able to combine two sounds in an interesting way that has potential creative applications. As such, the original problem statement has been fulfilled.

That being said the DDSP model did not provide very good results as far as accuracy of modeling are concerned.

Furthermore, the tone combinations produced by the model were not that impressive either.

In general, audio-generating models are highly cpu-intensive. More well-trained models would likely perform better.


Next Steps:
To improve these models given the framework I already have, I would simply train the autencoders for a few more days each, and drop their loss function for more accurate modeling.


To improve the outcome with a different set of models:
I am interested in creating a framework to apply CREPE to GanSynth. As the main reason I did not use GanSynth is because it required data that was tagged with pitch and loudness to model. The CREPE encoder automatically identifies pitch in raw audio for the model. As such, I would be able to develop a model with more specific control over sound combinations, more similar to ArtBreeder than the DDSP modeling, as GanSynth is a CNN-based model.

Finally,
With far  superior computing power WaveNet would have certainly performed orders of magnitudes better than any of the other models. However, WaveNet is extremely computationally expensive. When I have more access to greater computing power I will be sure to integrate WaveNet more.



**NOTE
Autencoders could not be included in GitHub repository because file sizes were too big to upload to github.









