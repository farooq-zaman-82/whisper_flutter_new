# Whisper Flutter New

[![pub package](https://img.shields.io/pub/v/whisper_flutter_new.svg?label=whisper_flutter_new&color=blue)](https://pub.dev/packages/whisper_flutter_new)

# Important Notice:

**I am currently refactoring the functionality and plan to provide flutter bindings for both llama.cpp and whisper.cpp. After the refactoring is complete, I will consider adapting to Windows and MacOS**

Ready to use [whisper.cpp](https://github.com/ggerganov/whisper.cpp) models implementation for iOS
and Android

1. Support AGP8+
2. Support Android 5.0+ & iOS 13+ & MacOS 11+
3. It is optimized and fast

Supported models: tiny、tiny.en (English only)、base、small、medium、large-v1、large-v2

Recommended Models：base、small、medium

All models have been actually tested, test devices: Android: Google Pixel 7 Pro, iOS: M1 iOS
simulator，MacOS: M1 MacBookPro & M2 MacMini

## Install library

```bash
flutter pub add whisper_flutter_new
```

## import library

```dart
import 'package:whisper_flutter_new/whisper_flutter_new.dart';
```

## Quickstart

```dart
// Prepare wav file
final Directory documentDirectory = await getApplicationDocumentsDirectory();
final ByteData documentBytes = await rootBundle.load('assets/jfk.wav');

final String jfkPath = '${documentDirectory.path}/jfk.wav';

await File(jfkPath).writeAsBytes(
    documentBytes.buffer.asUint8List(),
);

// Begin whisper transcription
// By default, tiny and tiny.en download from a Google Cloud Storage mirror
// (https://storage.googleapis.com/rppg-project-whisper-models); all other
// models download from Huggingface. Pass downloadHost to override, e.g.
// China: https://hf-mirror.com/ggerganov/whisper.cpp/resolve/main
final Whisper whisper = Whisper(
    model: WhisperModel.tinyEn, // English-only transcription
);

final String? whisperVersion = await whisper.getVersion();
print(whisperVersion);

final String transcription = await whisper.transcribe(
    transcribeRequest: TranscribeRequest(
        audio: jfkPath,
        isTranslate: true, // Translate result from audio lang to english text
        isNoTimestamps: false, // Get segments in result
        splitOnWord: true, // Split segments on each word 
    ),
);
print(transcription);
```
