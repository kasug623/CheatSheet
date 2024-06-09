# TOC
- [UniTask, UniRx](#unitask-unirx)

# URP, shaderGraph, DrawMeshInstancedIndirect
## How to handle "instancID" on shaderGraph
A node of "instanceID" doesn't work.  
For solving this problem, a dummy "custom function node" has to be created and this below sentence has to be set up on this dummy node.
```
#pragma instancing_options procedural:XXX
```
https://answers.unity.com/questions/1877154/urp-shader-graphs-with-instanced-indirect-instance.html


# Unity MacOS error: Drawing with MeshTopology.Points, yet the vertex program 'Hidden/OpaqueSelection' does not have PSIZE output.
Those warning messages are caused by the Scene View. It doesn't affect the quality of rendering. Please ignore them.
[https://github.com/keijiro/Pcx/issues/71](https://github.com/keijiro/Pcx/issues/71)  

# region
エディタ上で折りたたむ（一時的に非表示）ことができる。
```
#region 名称

コードコードコードコードコードコードコード
コードコードコードコードコードコードコード
コードコードコードコードコードコードコード
コードコードコードコードコードコードコード

#endregion
```
[https://icoc-dev.hatenablog.com/entry/2014/01/15/090644](https://icoc-dev.hatenablog.com/entry/2014/01/15/090644)  

# シェーダ上で使える予約済み変数
```
float3	_WorldSpaceCameraPos; //ワールド座標系のカメラの位置
float4	_Time; //時間 (t/20、t、t×2、t×3)、シェーダの中でアニメーションするのに使用

```
[https://qiita.com/edo_m18/items/591925d7fc960d843afa](https://qiita.com/edo_m18/items/591925d7fc960d843afa)

# HLSLで使える関数
[https://hlslref.wiki.fc2.com/](https://hlslref.wiki.fc2.com/)  

# .cgincの　#ifndef, #define, #endif
```
#ifndef EXAMPLE_INCLUDED
#define EXAMPLE_INCLUDED

// ここに処理を書いていく

#endif
```
二重インクルードの防止。EXAMPLE_INCLUDEDが存在すれば、IF文発動でスキップなければ、DEFINEで定義されて、以下のコードが実行。次回からはDEFINE済みなのでIF文発動でスキップ。よって一回だけインクルードができる。
[https://light11.hatenadiary.com/entry/2019/01/09/233507](https://light11.hatenadiary.com/entry/2019/01/09/233507)  
[https://www.kinjo-u.ac.jp/ushida/tekkotsu/cpp/ifndef.html](https://www.kinjo-u.ac.jp/ushida/tekkotsu/cpp/ifndef.html) 

# スクリプト上でMaterialをアタッチ
```
Material material = new Material(Shader.Find("Transparent/Diffuse"));
```
[https://docs.unity3d.com/jp/current/ScriptReference/Shader.Find.html](https://docs.unity3d.com/jp/current/ScriptReference/Shader.Find.html)

# # pragma target 3.5 の意味
2.5: derivatives  
3.0: 2.5 + interpolators10 + samplelod + fragcoord  
3.5: 3.0 + interpolators15 + mrt4 + integers + 2darray + instancing  
4.0: 3.5 + geometry  
5.0: 4.0 + compute + randomwrite + tesshw + tessellation  
4.5: 3.5 + compute + randomwrite  
4.6: 4.0 + cubearray + tesshw + tessellation  
[https://docs.unity3d.com/ja/2018.4/Manual/SL-ShaderCompileTargets.html](https://docs.unity3d.com/ja/2018.4/Manual/SL-ShaderCompileTargets.html)

# Macでは、ジオメトリシェーダーとテッセレーションシェーダーは使えない
Apple が OS X デスクトップ上の OpenGL バージョン を最大で 4.1 までと制限したため、すべての DirectX 11 機能 (Unordered Access Views や コンピュートシェーダーなど) に対応しているわけではありません。つまり、シェーダーレベル 5.0 ( #pragma target 5.0を伴う) に対応するように設定しているすべてのシェーダーは OS X でロードできない
[https://docs.unity3d.com/ja/2018.4/Manual/OpenGLCoreDetails.html](https://docs.unity3d.com/ja/2018.4/Manual/OpenGLCoreDetails.html)

# OnRenderObject() は予約済み関数
OnRenderObject is called after camera has rendered the Scene.  
This can be used to render your own objects using Graphics.DrawMeshNow or other functions. 
[https://docs.unity3d.com/ja/current/ScriptReference/MonoBehaviour.OnRenderObject.html](https://docs.unity3d.com/ja/current/ScriptReference/MonoBehaviour.OnRenderObject.html)

# Random.insideUnitSphere 関数の動き
半径 1 の球体の内部のランダムな点を返す（読み取り専用）
[https://docs.unity3d.com/ja/2018.4/ScriptReference/Random-insideUnitSphere.html](https://docs.unity3d.com/ja/2018.4/ScriptReference/Random-insideUnitSphere.html)

# UniTask, UniRx
## how to intall on Windows
1. install Git on Windows
2. add git path to PATH
3. add UniTask and UniRx URL to PackageManager


# know-how
## faced error : [Spawn]-[Laser] problem
using compute shader and add new compute buffer to .compute with copy-and-paste,
I forgot to change stuct type for the buffer.

## VisualStudioを使ったデバック機能が機能しない
起動したら治った。後そもそもスクリプトをアタッチしていなかった。	