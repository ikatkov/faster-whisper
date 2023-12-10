# Multilingual Dictation App based on OpenAI Whisper

## Change

This is a fork of https://github.com/foges/whisper-dictation

I've replaced the openAI's whisper with https://github.com/SYSTRAN/faster-whisper

It does work. With CPU only - I can see x2.3 speed increase. You can use medium model with acceptable time lag.

The ugly part is that faster-whisper uses `ctranslate2` which needs `libiomp5.dylib` and `openai-whisper` uses `torch` which needs `libiomp5.dylib`, but these are different `libiomp5` 

And the code fails at run-time.

```
OMP: Error #15: Initializing libiomp5.dylib, but found libiomp5.dylib already initialized.
OMP: Hint This means that multiple copies of the OpenMP runtime have been linked into the program. That is dangerous, since it can degrade performance or cause incorrect results. The best thing to do is to ensure that only a single OpenMP runtime is linked into the process, e.g. by avoiding static linking of the OpenMP runtime in any library. As an unsafe, unsupported, undocumented workaround you can set the environment variable KMP_DUPLICATE_LIB_OK=TRUE to allow the program to continue to execute, but that may cause crashes or silently produce incorrect results. For more information, please see http://www.intel.com/software/products/support/
```


The code below does the ugliest hack possible by installing/uninstalling `torch`.

To use with OpenAI lib, use `-i openai` (default), to use faster-whisper do`-i fast-whisper`. 

Example `python whisper-dictation.py -m medium -i fast-whisper`

## Original

Multilingual dictation app based on the powerful OpenAI Whisper ASR model(s) to provide accurate and efficient speech-to-text conversion in any application. The app runs in the background and is triggered through a keyboard shortcut. It is also entirely offline, so no data will be shared. It allows users to set up their own keyboard combinations and choose from different Whisper models, and languages.

## Prerequisites
The PortAudio library is required for this app to work. You can install it on macOS using the following command:

```bash
brew install portaudio
```

## Permissions
The app requires accessibility permissions to register global hotkeys and permission to access your microphone for speech recognition.

## Installation
Clone the repository:

```bash
git clone https://github.com/foges/whisper-dictation.git
cd whisper-dictation
```

Create a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

## Usage
Run the application:

```bash
python whisper-dictation.py
```

By default, the app uses the "base" Whisper ASR model and the key combination to toggle dictation is cmd+option on macOS and ctrl+alt on other platforms. You can change the model and the key combination using command-line arguments.  Note that models other than `tiny` and `base` can be slow to transcribe and are not recommended unless you're using a powerful computer, ideally one with a CUDA-enabled GPU. For example:


```bash
python whisper-dictation.py -m large -k cmd_r+shift -l en
```

The models are multilingual, and you can specify a two-letter language code (e.g., "no" for Norwegian) with the `-l` or `--language` option. Specifying the language can improve recognition accuracy, especially for smaller model sizes.

## Setting the App as a Startup Item
To have the app run automatically when your computer starts, follow these steps:

 1. Open System Preferences.
 2. Go to Users & Groups.
 3. Click on your username, then select the Login Items tab.
 4. Click the + button and add the `run.sh` script from the whisper-dictation folder.
