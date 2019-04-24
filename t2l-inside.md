---
#### Detailed description about connection algorithms used in t2l
---

> Actual connection window evaluated from runcard settings can be obtained using a program,  
> ```linklet_window pos1 pos2 [event-descriptor](event-descriptor.md) rc --geom 0/1```  

---

+ parameters used to calculate connection window, defined in runcard t2l.rc
  > Mode = 0(set by values, ConnectLinklet) / 1(set by minimum momentum and sigma, IConnectLinklet)  
  > CircleCut = 0(square) / 1(circle)  
  > RadialCut = 0(x-y,default) / 1(raadial-lateral)  
  > Errors1 = ErrX1 ErrY1 ErrAx1 ErrAy1 ErrCx1 ErrCy1 ErrCax1 ErrCay1 (errors for 1st track)  
  > Errors2 = ErrX2 ErrY2 ErrAx2 ErrAy2 ErrCx2 ErrCy2 ErrCax2 ErrCay2 (errors for 2nd track)  
  > MinimumMomentum = pmin sigma ( used only for Mode=1 )  

+ connection window evaluation
  > ErrMomAng : 運動量下限から求めた角度窓 ( １シグマ相当 )  
  > ErrMomPos : 運動量下限から求めた位置窓 ( １シグマ相当 )  
  > zp        : 運動量下限から求めた投影 Z 面  
  >  
  > Mode = 1  
  >>  運動量下限からの位置・角度窓の求め方  
  >>  運動量 pmin, 角度 0 のトラックに対して、  
  >>  投影 Z 面を [z1,z2] の間を 100 分割した各 Z に置き、  
  >>  それぞれの 投影 Z 面での多重散乱起因の位置・角度の rms を 1000 回のモンテカルロ計算で求める。  
  >>  そして、与えられた Errors1,2,3,4 を加味し、位置エラーが最小となる投影 Z 面 (zp) を求める。  
  >>  その Z 面での多重散乱起因の位置・角度の rms を ErrMonPos,ErrMonAng とする。  
  >>  
  >>  s = sqrt(2) &times; sigma  
  >>  

  > Mode = 0  
  >>  ErrMomAng = 0  
  >>  ErrMomPos = 0  
  >>  zp = (1-f)*z1+f*z2 ( f is Linklet::Zproj in runcard or 0.5 )  
  >>  s = 1  
  >>

  > z1,z2 : 接続を試みる２面それぞれの Z 座標 ( BaseTrack の場合は、相互に近い側の乳剤ベース面 )  
  > ax,ay : 接続を試みるトラックの平均角度の絶対値  
  >  
  > dz1 = fabs( z1-zp )  
  > dz2 = fabs( z2-zp )  
  >  
  > eax = s &times; sqrt( (ErrAx1+ErrCax1 &times; ax)<sup>2</sup> + (ErrAx2+ErrCax2 &times; ax)<sup>2</sup> + (ErrMomAng)<sup>2</sup> &times; w )  
  > eay = s &times; sqrt( (ErrAy1+ErrCay1 &times; ay)<sup>2</sup> + (ErrAy2+ErrCay2 &times; ay)<sup>2</sup> + (ErrMomAng)<sup>2</sup> &times; w )  
  >> w &equiv; sqrt( 1.0 + min(ax<sup>2</sup>+ay<sup>2</sup>,1.0) ) &larr; トラックの角度により、通過経路が長くなる効果を補正するための係数。
  >  
  > ex = s &times; sqrt( (ErrX1+ErrCx1 &times; ax )<sup>2</sup> + (dz1 &times; (ErrAx1+ErrCax1 &times; ax))<sup>2</sup> + (ErrX2+ErrCx2 &times; ax)<sup>2</sup> + (dz2 &times; (ErrAx2+ErrCax2 &times; ax))<sup>2</sup> + ErrMomPos<sup>2</sup> &times; w<sup>3</sup> )  
  > ey = s &times; sqrt( (ErrY1+ErrCy1 &times; ay )<sup>2</sup> + (dz1 &times; (ErrAy1+ErrCay1 &times; ay))<sup>2</sup> + (ErrY2+ErrCy2 &times; ay)<sup>2</sup> + (dz2 &times; (ErrAy2+ErrCay2 &times; ay))<sup>2</sup> + ErrMomPos<sup>2</sup> &times; w<sup>3</sup> )  
  >  
  > これら eax,eay,ex,ey を使い、下記で接続判定を行う。  

  > RadialCut = 0  
  >>  &delta;x,&delta;y : position difference at z = zp  
  >>  &delta;&theta;x,&delta;&theta;y : angle difference  
  >>
  >>  Runcard values in [Linklet] used in calculation below.  
  >>  WindowMin = wxmin wymin waxmin waymin  
  >>  WindowMax = wxmax wymax waxmax waymax  
  >>  
  >>  CircleCut = 0 : waxmin-eax &le; &delta;&theta;x &le; waxmax+eax && waymin-eay &le; &delta;&theta;y &le; waymax+eay && wxmin-ex &le; &delta;x &le; wxmax+ex && wymin-ey &le; &delta;y &le; wymax+ey  
  >>  CircleCut = 1 : (&delta;&theta;x/(waxmax+eax))<sup>2</sup> + (&delta;&theta;y/(waymax+eay))<sup>2</sup> &le; 1 && (&delta;x/(wxmax+ex))<sup>2</sup>+(&delta;y/(wymax+ey))<sup>2</sup> &le; 1  

  > RadialCut = 1  
  >> x を radial, y を lateral に読み替えて計算する。CircleCut の値に応じて RadialCut = 0 の場合と同様の接続判定を行う。  

---
#### Linklet::MinimumMomentum に関するメモ ( 不要な計算？ )
+ t2l ( IConnectLinklet ) では、運動量の逆数 ( 1/p ) 空間で 0 ~ 1/pmin を 19 分割し、  
  各 linklet がどの区画から接続されるかを求め、これ ( 接続可能な最低運動量 ) を Linklet::m_MomCut に記録している。  
  ここで pmin は runcard の Linklet::MinimumMomentum の $1 であるが、$2 (sigma) は接続窓の計算で使われるため、  
  記録される接続可能な最低運動量はその sigma での値である。  
  m_MomCut へのアクセスには Linklet::GetMomCut() を使う事。  

---
#### 要改善点
+ sigma は運動量起因の窓にのみ適用すべきではないか？
+ 位置窓の計算に、角度エラーは入れない方が使い易くないか？
+ Mode=1 で、面間隔の nominal 値からのズレを、散乱物質に反映させるのが困難なので、  
  各面のトラックを一度 nominal な Z 面に投影した後、その間でつなぎを行なっているが、  
  この為、nominal z からのズレの部分での角度誤差起因の位置エラーが考慮されていない。  
