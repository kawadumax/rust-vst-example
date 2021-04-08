# 20210323

ice を hatoo さんのブランチを持ってきてやろうと思ったけど時間が来てしまった。
hatoo さんが winit-0.2.1 のブランチを消してる気がする

# 20210326

今はこれを使うのが主流とのこと。
https://github.com/BillyDM/iced_baseview
https://github.com/BillyDM/iced-baseplug-examples

# 20210401

ひさびさに開発を続ける。方針が経たないなぁ...
baseplug を使わずに iced_baseview だけかます方法を知りたい

- [] Editor トレイトを実装したらいいのかな

# 20210402

vst クレートに GUI を渡す必要があるんだけど、サンプルコードだと baseplug 経由になっていて、vst が隠ぺいされている。
ので、baseplug のコードを読もう。
読んでもよくわからなかった。

# 20210404

VST 側から GUI プログラムに要求されているのは、Editor というトレイトを実装することだ。
そして、GUI プログラムは Application というトレイトを実装する。
hatoo さんの記事ではこの関係はどうなっていたかというと
GUI 構造体があって、GUIwrapper 構造体がそれを所有している。
GUI 構造体は new を impl されている（その中で、後述する WhisperGui が new されている。）。
GUIwrapper に対して Editor トレイトは実装されている。
WhisperGui 構造体があって、そこに対して iced の Application は実装されている。

これを iced_baseview に対して行おう。

なんか別に「聴けばよくね」って思ったので

```
Hi, I am writing vst-rs with iced-baseview.
I am having a hard time grasping the whole idea of building vst using baseview.
The only sample I could find was the one using baseplug. Since the plugin part is already implemented, I am trying to find a way to build vst without using baseplug.
Can we build vst with iced_baseview, but without baseplug? Sorry if this is a silly question.
```

を discord コミュニティにぶち込んでみた。
hub で酒を飲んでいるからこそできることかもしれないね。

もう少し読み込もう。
なるべく自分の力で書こう。

# 20210405

昨日のやつの返信が来てた。

```
@toa Hi :slight_smile: so baseview is just a windowing backend for iced to allow creating windows with a parent window handle. This it required for a vst but there's nothing vst specific about baseview. Baseplug on the other hand is a lib for creating a vst, providing macros and methods to manipulate the audio data provided by a daw. If you don't want to use baseplug for whatever reason, there is also the vst crate that does the same thing but is a lot more manual than baseplug. There used to be some examples using iced_baseview and vst crate but I can't seem to find them. Probably because baseplug is the better crate to use. Paging @BillyDM for clarification.
```

````
@toa Hello! Here's the example of using iced with baseplug/baseview. https://github.com/BillyDM/iced-baseplug-examples
GitHub
BillyDM/iced-baseplug-examples
Simple examples to demonstrate full-stack Rust audio plugin dev with baseplug and iced_audio - BillyDM/iced-baseplug-examples

There are also examples of how to use baseview with the vst crate here. The setup should be the same for iced, just replace the egui/imgui library with the iced one. https://github.com/DGriffin91/imgui_baseview_test_vst2 https://github.com/DGriffin91/egui_baseview_test_vst2
GitHub
DGriffin91/imgui_baseview_test_vst2
Barebones imgui_baseview vst2 plugin with basic parameter control - DGriffin91/imgui_baseview_test_vst2

GitHub
DGriffin91/egui_baseview_test_vst2
Barebones egui_baseview vst2 plugin with basic parameter control - DGriffin91/egui_baseview_test_vst2

Oh yeah, keep in mind that baseplug still does not have an official way to tie parameters between the GUI and the audio thread, and thus the iced-baseplug-examples example does not have that hooked up. You currently have to do that manually with your own system for now.```

````

返信を書こう。

```
@geom3trik @BillyDM Thank you very much for your kind attention. I'm glad that I'm about to solve something that I've been struggling with alone for a long time. I'll give it a try.

```

# TODO

- [] とりあえずサンプルを読む。
