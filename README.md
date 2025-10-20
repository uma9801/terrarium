JavaScriptでテラリウムを作る from Web-Deb-For-Beginners

2025/10/19  
新機能を追加  
// ダブルクリック時に、対象の画像を最前面にする  
初期は function pointerDrag(e) 内に再度 terrariumElement.onpointerdown = の判定を設けてダブルクリック判定をしようとしたが、マウスボタン押下の処理が上書きされるだけで、ダブルクリック判定にならなかった（移動した画像を再度移動することもできなくなった）  
そこから terrariumElement.ondblclick に変えることでダブルクリック判定を可能にして最前面表示が実装できた  
- その他初使用のもの： parseInt, querySelectorAll, window.getComputedStyle(elements[i]).zIndex, isNaN

2025/10/17  
02  
DOM & Closureで画像のD&Dを可能にしたが、バニラindex.htmlでは正常動作したものの、Flexbox・Gridで変更したindex.htmlは画像を動かせなかった（D&Dの座標は拾えている）
画像そのものをFlexboxで配置しているため、位置が固定されていると仮説する。
- 位置が固定されているのは正しかったが、Flexboxが原因ではなくpositionを指定しておらずデフォルトのstatic（位置固定）になっていたため。しかし、absoluteにすれば画像が重なり背景も消え、relativeにするとD&Dした瞬間に激しい位置ずれが起こる（整列と背景色適応はされる）
消した.plant-holderを戻し、そこにFlexboxを設置（.plant-holderがContainer、.plantがItems）して.plant-holderをrelative、.plantをabsoluteにすることで配置と画像移動を解決
強いて言えば、後は画像の大きさ（公式はもっと大きい）ただしこれは等間隔に配置するか重なりを許容するかの違いとも言える。さらに言えば公式は背景色をなくしていて分からないが、Containerに対しての中央寄せはできていないと思われる。
01  
.plantを1画像ごとに.plant-holderで囲っていたのを左右container毎の囲いに修正しFlexboxを適用。flex-direction: column;で縦並びに、.container imgのheightを13%にして画像サイズ調整を実施。不要になった.plantの画像サイズ上限を削除し、.plant-holderも削除
ボトルと左右コンテナの間隔調整の為に.containerのmarginをautoにした

2025/10/16  
Grid Layoutで左右のContainerと中央terrariumをエリア分けし、左右のContainerの範囲が適切になった。ウィンドウの変化にも対応。absolute, relativeなどの設定も不要
plant-holderもFlexboxで整えようとしたが、上手く行かなかった
- 現状は重ならない等間隔での整列
以下、教訓
- Gridは直下タグでないと反映されない（#bodyで指定しても、ネストが2の#Left-containerでは無効）
- .containerで新たにGrid(7, 1fr)で作成して直下の.plant-holderをFlexboxで整列させようとしても、1つのGrid内で.plantが格納されて画像が全て並んでしまい、1つのGridずつに画像を配置するという事はできなかった。また、Containerを縦7つのGridには分けられたが、height設定をしないと画像が原寸大になってしまう（想定のGrid範囲を突き抜ける）