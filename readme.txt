#htk-speaker-recognition

this is an implementation of a single utterance recognition system using HTK. Several recordings were taken of speakers uttering the same phrase. The system analyses the grammar, phrases and utterances to determine if a new recording belongs to the target speaker or an impostor.


##Conventions:
All speakers data is stored as follows

   username/sample01.wav
   username/sample02.wav
   ...
   username/sample10.wav

All Samples were recorded with - [Audacity for Windows](http://audacity.sourceforge.net/download

Settings:
Number of Samples: 	10
Sample Rate: 		16000Hz
Bit Rate:		16bit
Channels:		Mono
Phrase:			A boring novel is a superb sleeping pill.

##HTK Commands

These are the commands that i ran alongside the HTK Tutorial located in the references. 
Any extra info that wasn't clear i found (eventually in the HTK book that referenced in the tutorial)

``` bash
HParse gram.txt wdnet

HCopy -T 1 -C config_wav2mfc -S convert.scp

HList Data/ma1.mfc

mkdir hmm0
HCompV -C config_mfc -f 0.01 -m -S training.scp -M hmm0 proto

mkdir hmm1
HERest -C config_mfc -I speakertrainmodels0.mlf -S training.scp -H hmm0/macros -H hmm0/hmmdefs -M hmm1 phonemodels0
mkdir hmm2
HERest -C config_mfc -I speakertrainmodels0.mlf -S training.scp -H hmm1/macros -H hmm1/hmmdefs -M hmm2 phonemodels0
mkdir hmm3
HERest -C config_mfc -I speakertrainmodels0.mlf -S training.scp -H hmm2/macros -H hmm2/hmmdefs -M hmm3 phonemodels0
```

# Test a speaker

``` bash
HVite -H hmm3/macros -H hmm3/hmmdefs -S testing.scp -i results_speaker.mlf -w wdnet dict HmmList
HResults -I speakertestmodels0.mlf HmmList results_speaker.mlf 
```

# All Results for speaker

``` bash
HVite -H hmm3/macros -H hmm3/hmmdefs -S training.scp -i results_speaker.mlf -w wdnet dict HmmList
HResults -I speakertrainmodels0.mlf HmmList results_speaker.mlf 
```

# Imposters

``` bash
HVite -H hmm3/macros -H hmm3/hmmdefs -S ImposterTestAll.scp -i results_imposter.mlf -w wdnet dict HmmList
HResults -I ImposterTestmodelAll.mlf HmmList results_imposter.mlf
```
