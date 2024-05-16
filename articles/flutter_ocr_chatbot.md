---
title: "FlutterでChatGPT-4oを使用した日本語OCR機能付きチャットボットアプリの作成"
emoji: "📚"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Flutter", "ChatGPT", "OCR", "Mobile Development"]
published: false
---

この記事では、OpenAIが2024年5月に発表したChatGPT-4oを利用して、日本語のOCR機能を持つチャットボットアプリをFlutterで開発する手順を解説します。FlutterはGoogleが開発したモバイル、ウェブ、デスクトップアプリケーションの開発が可能なフレームワークで、一度の開発で複数のプラットフォームに対応するアプリを作成できます。今回は、その中でも特にiOSとAndroidのモバイルアプリケーションの開発に焦点を当てます。

# 前提条件

- Flutterの基本的な知識と環境設定が完了していること
- Dart言語についての基本的な知識
- OpenAIのAPIキーを取得していること

# 必要なツールとライブラリ

1. **Flutter SDK**: 最新版を[公式サイト](https://flutter.dev/)からインストールしてください。
2. **Android Studio or Visual Studio Code**: 開発環境として、どちらかを用意してください。
3. **openai_flutter**: OpenAIのChatGPTモデルと通信するためのFlutter用ライブラリです。
4. **google_ml_kit**: GoogleのML Kitを使用して画像からテキストを認識するOCR機能を追加します。

# アプリケーションの設計

アプリケーションは大きく三つの部分で構成されます。

1. **OCR機能**: ユーザーがカメラまたはギャラリーから画像を選択し、その画像のテキストを抽出します。
2. **ChatGPT-4oの統合**: 抽出したテキストを使用して、ChatGPT-4oと対話します。
3. **UI**: ユーザーが容易に操作できるシンプルなUIをFlutterで実装します。

# 開発手順

## Step 1: Flutterプロジェクトのセットアップ

```bash
flutter create ocr_chatbot
cd ocr_chatbot
```

## Step 2: 必要なライブラリのインストール

`pubspec.yaml`に以下を追加します。

```yaml
dependencies:
  flutter:
    sdk: flutter
  openai_flutter: ^0.1.0  # ChatGPT用ライブラリ
  google_ml_kit: ^0.10.0  # OCR機能用ライブラリ
```

## Step 3: OCR機能の実装

`lib/ocr_service.dart`ファイルを作成し、以下のように実装します。

```dart
import 'package:google_ml_kit/google_ml_kit.dart';

class OcrService {
  final textRecognizer = TextRecognizer(script: TextRecognitionScript.japanese);

  Future<String> extractTextFromImage(InputImage inputImage) async {
    final recognizedText = await textRecognizer.processImage(inputImage);
    return recognizedText.text;
  }
}
```

## Step 4: ChatGPTとの通信設定

`lib/chat_service.dart`ファイルを作成し、以下のコードを記述します。

```dart
import 'package:openai_flutter/openai_flutter.dart';

class ChatService {
  final _api = OpenAI(apiKey: 'YOUR_API_KEY');

  Future<String> send

TextToChatGPT(String text) async {
    final response = await _api.completions.create(
      engineId: 'text-davinci-004',  # ChatGPT-4oのエンジンIDを指定
      prompt: text,
      maxTokens: 150,
    );
    return response.choices.first.text;
  }
}
```

## Step 5: UIの構築

ここでは、OCR機能とChatGPTを利用するチャットボットアプリのUIを構築する方法を詳しく説明します。ユーザーが画像をアップロードでき、画像からテキストを抽出してそのテキストを用いてChatGPTと対話できるようなシンプルなUIを作成します。

### 必要なファイルと設定

- `main.dart`: Flutterアプリのエントリポイント
- `ocr_service.dart`: OCR機能を提供（前のステップで作成）
- `chat_service.dart`: ChatGPTとの通信を管理（前のステップで作成）

### `main.dart`のコード

Flutterの`main.dart`には、以下のように記述します。このコードは、アプリケーションのUI部分を構築するためのものです。

```dart
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';
import 'ocr_service.dart';
import 'chat_service.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'OCR Chatbot',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final ImagePicker _picker = ImagePicker();
  final OcrService _ocrService = OcrService();
  final ChatService _chatService = ChatService();
  String _extractedText = '';
  String _responseText = 'ChatGPT Response will appear here.';

  Future<void> _pickImage() async {
    final XFile? image = await _picker.pickImage(source: ImageSource.gallery);
    if (image != null) {
      final extractedText = await _ocrService.extractTextFromImage(image.path);
      setState(() {
        _extractedText = extractedText;
      });
      _sendTextToChatGPT(extractedText);
    }
  }

  Future<void> _sendTextToChatGPT(String text) async {
    final response = await _chatService.sendTextToChatGPT(text);
    setState(() {
      _responseText = response;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('OCR Chatbot'),
      ),
      body: SingleChildScrollView(
        child: Column(
          children: <Widget>[
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Text('Extracted Text: $_extractedText'),
            ),
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Text('ChatGPT Response: $_responseText'),
            ),
            ElevatedButton(
              onPressed: _pickImage,
              child: Text('Pick Image and Extract Text'),
            ),
          ],
        ),
      ),
    );
  }
}
```

このコードは、以下の機能を実装しています：

1. **画像の選択**: ユーザーがギャラリーから画像を選択できます。
2. **テキストの抽出**: 選択された画像からテキストを抽出します。
3. **ChatGPTとの対話**: 抽出したテキストを用いてChatGPTに問い合わせ、レスポンスを表示します。

このUIは基本的なものですが、実用的なチャットボットアプリのプロトタイプとして機能します。さらに機能を追加したり、デザインを洗練させたりすることで、よりユーザーフレンドリーなアプリケーションに発展させることができます。

## Step 6: テストとデバッグ

エミュレーターまたは実デバイスでアプリを実行し、OCRとChatGPTが正しく機能しているか確認します。

# まとめ

この記事では、Flutterを使ってOCR機能を持つChatGPT-4oを統合したチャットボットアプリの基本的な開発手順を説明しました。さらに詳しいカスタマイズや機能追加については、各ライブラリのドキュメントを参照してください。このようなアプリ開発により、ユーザーのニーズに合わせた対話型サービスを提供することが可能です。