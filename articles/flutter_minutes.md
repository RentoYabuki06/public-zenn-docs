---
title: "【話者分離付き】Flutterで議事録作成アプリを作る！"
emoji: "🐡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

Flutterを使って話者分離機能を持つ音声認識議事録作成アプリを開発するプロジェクトは、非常に革新的で興味深い取り組みです。この記事では、Flutterでのアプリ開発のステップを、必要なライブラリやAPIの利用方法とともに詳細に解説します。

# 概要

話者分離機能を備えた音声認識アプリは、複数人が同時に話している環境での会話を議事録として記録する場合に非常に有効です。この機能により、それぞれの話者が発言した内容を識別し、議事録にきちんと反映させることができます。

# 必要なツールとライブラリ

1. **Flutter**: モバイルアプリ開発フレームワーク。
2. **Dart**: Flutterのプログラミング言語。
3. **Firebase**: 認証やデータ保持に使用。
4. **Google Cloud Speech-to-Text API**: 高精度な音声認識機能を提供。
5. **TensorFlow Lite**: モバイルデバイス向けの軽量機械学習ライブラリ。話者分離モデルの実行に使用。

# 開発プロセス

## ステップ 1: Flutterプロジェクトの設定

```bash
flutter create speaker_recognition_app
cd speaker_recognition_app
```

## ステップ 2: 必要なパッケージのインストール

`pubspec.yaml`ファイルに以下の依存関係を追加します。

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^1.10.0
  firebase_auth: ^3.3.4
  google_speech: ^2.1.0
  tensorflow_lite_flutter: ^0.1.0
```

## ステップ 3: 認証機能の実装

Firebaseを使用してユーザ認証機能を実装します。この機能はユーザがアプリにログインして使用するために必要です。

```dart
import 'package:firebase_auth/firebase_auth.dart';

void signInWithEmail(String email, String password) async {
  try {
    final UserCredential userCredential = await FirebaseAuth.instance.signInWithEmailAndPassword(
      email: email,
      password: password,
    );
    print("ログイン成功: ${userCredential.user}");
  } catch (e) {
    print("ログインエラー: $e");
  }
}
```

## ステップ 4: 音声認識と話者分離機能の実装

Google Cloud Speech-to-Text APIとTensorFlow Liteを組み合わせて、音声データからテキストへの変換と話者分離を行います。

```dart
import 'package:google_speech/google_speech.dart';

void transcribeAudio() async {
  final speechToText = SpeechToText.viaSubscription("<YOUR_API_KEY>", "<YOUR_REGION>");
  final response = await speechToText.recognize(
    config: RecognitionConfig(
      encoding: AudioEncoding.LINEAR16,
      model: 'video',
      enableSpeakerDiarization: true,
      diarizationSpeakerCount: 2,
    ),
    audio: AudioContent(yourAudioBytes),
  );
  print("認識結果: ${response.results}");
}
```
確かに、Flutterでの完全な音声認識議事録作成アプリに必要なコードを追記しましょう。ここでは、ユーザー認証、音声認識、話者分離機能までをカバーする一連のコードを提供します。

# Flutter アプリの全コード

## パッケージのインポートと初期設定

まず、FirebaseとGoogle Speech APIを使うために必要なパッケージをインポートし、Firebaseを初期化します。

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:google_speech/google_speech.dart';
import 'package:audio_session/audio_session.dart';
import 'package:record/record.dart';
import 'package:path_provider/path_provider.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

## アプリの基本構造

`MyApp` クラスで基本的なUIとナビゲーションを設定します。

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Speaker Recognition App',
      home: HomePage(),
    );
  }
}
```

## ホームページの実装

ユーザーがログインし、録音を開始できるようにします。

```dart
class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final FirebaseAuth _auth = FirebaseAuth.instance;
  bool _isRecording = false;
  final _recorder = Record();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Speaker Recognition App'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            ElevatedButton(
              onPressed: () async {
                if (_auth.currentUser == null) {
                  await signInWithEmail("test@example.com", "password");
                }
                if (!_isRecording) {
                  startRecording();
                } else {
                  stopRecording();
                }
              },
              child: Text(_isRecording ? 'Stop Recording' : 'Start Recording'),
            ),
          ],
        ),
      ),
    );
  }

  void startRecording() async {
    final bool result = await _recorder.start();
    setState(() {
      _isRecording = result;
    });
  }

  void stopRecording() async {
    final path = await _recorder.stop();
    setState(() {
      _isRecording = false;
    });
    transcribeAudio(path);
  }
}
```

## Firebase 認証と Google Cloud Speech-to-Text

ログイン機能と音声認識の実装を追加します。

```dart
Future<void> signInWithEmail(String email, String password) async {
  try {
    await _auth.signInWithEmailAndPassword(email: email, password: password);
  } catch (e) {
    print("Login Error: $e");
  }
}

void transcribeAudio(String path) async {
  final bytes = await File(path).readAsBytes();
  final speechToText = SpeechToText.viaSubscription("YOUR_API_KEY", "YOUR_REGION");
  final response = await speechToText.recognize(
    config: RecognitionConfig(
      encoding: AudioEncoding.LINEAR16,
      sampleRateHertz: 16000,
      languageCode: 'ja-JP',
      enableSpeakerDiarization: true,
      diarizationSpeakerCount: 2,
    ),
    audio: AudioContent(bytes),
  );

  for (var result in response.results) {
    print("認識結果: ${result.alternatives.first.transcript}");
  }
}
```

# まとめ

これで、Firebaseを使ったユーザー認証、録音の開始・停止、そしてGoogle Cloud Speech-to-Textを利用した音声認識と話者の分離機能を持つ基本的なFlutterアプリのコードが完成しました。サンプルとして使用するAPIキーやログイン情報は、実際の運用ではセキュリティを考慮して適切に管理してください。