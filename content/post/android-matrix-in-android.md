+++
mathjax = true
title = "Matrix in Android"
date = "2016-01-12"
tags = ["android", "math"]
categories = ["android"]
+++
许多时候我们在数学课本上学的可能更多是知其然而不知其所以然，然而要理解一个概念可能只需要一个观点才不至于被搞懵，比如从应用的角度出发把线性代数当作是一种人为设计的**领域特定语言**（ domain specific language ）可能更容易被程序员们所接受。具体参考：[程序观点下的线性代数](http://www.cnblogs.com/weidagang2046/p/linear-algebra-from-programming-perspective.html)。


下面先简单介绍（复习）矩阵的定义、矩阵的运算，然后才是在 android 中使用矩阵

### 矩阵的定义

> 由 $m\times n$ 个数 $a_{ij}(i=1,2,\dots,m;j=1,2,\dots,n)$ 排成的 m 行 n 列的数表:
>
> <div>
> $$
>    \begin{matrix}
>    a_{11} & a_{12} & \dots & a_{1n}\\
>    a_{21} & a_{22} & \dots & a_{2n}\\
>    \vdots & \vdots & & \vdots\\
>    a_{m1} & a_{m2} & \dots & a_{mn}
>    \end{matrix}
> $$
> </div>
>
> 称为 m 行 n 列矩阵。

人们常常使用大写字母来表示矩阵，如

<div>
$$
A=\begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n}\\
a_{21} & a_{22} & \cdots & a_{2n}\\
\vdots & \vdots & &\vdots\\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix}
$$
</div>


$a_{ij}(i=1,2,\cdots,m; j=1,2, \cdots,n)$ 即为矩阵 **A** 中第 i 行第 j 列的元素。

### 矩阵的运算

#### 一、矩阵的加法

> 设有两个 $m \times n$ 矩阵 **A**=($a\_{ij}$) 和 **B**=($b\_{ij}$) ，那么矩阵 **A** 与矩阵 **B** 的和记为 **A** + **B** ，规定为
>
> <div>
> $$
> \mathbf{A}+\mathbf{B}=\begin{bmatrix}
> a_{11}+b_{11} & a_{12}+b_{12} & \cdots & a_{1n}+b_{1n}\\
> a_{21}+b_{21} & a_{22}+b_{22} & \cdots & a_{2n}+b_{2n}\\
> \vdots & \vdots & & \vdots\\
> a_{m1}+b_{m1} & a_{m2}+b_{m2} & \cdots & a_{mn}+b_{mn}
> \end{bmatrix}
> $$
> </div>
>
> ，即 **A**+**B**=($a\_{ij}$ + $b\_{ij}$)


矩阵的加法满足以下规律

* 交换律：**A** + **B** = **B** + **A**
* 结合律：**A** + (**B** + **C**) = (**A** + **B**) + **C**
* 存在零元：**A** + **0** = **0** + **A**
* 存在负元：**A** + (-**A**) =(-**A**) + **A** = **0**

#### 二、数与矩阵相乘

> 数 λ 与矩阵 **A** 的乘积记作 λ**A** 或 **A**λ ，规定为
> 
> <div>
> $$
> λ\mathbf{A}=\mathbf{A}λ=\begin{bmatrix}
> λa_{11} & λa_{12} & \cdots & λa_{1n}\\
> λa_{21} & λa_{22} & \cdots & λa_{2n}\\
> \vdots & \vdots & & \vdots\\
> λa_{m1} & λa_{m2} & \cdots & λa_{mn}
> \end{bmatrix}
> $$
> </div>

数乘矩阵满足以下运算规律（设 **A** , **B** 为 $m \times n$ 矩阵， λ ， μ 为数）

* 交换律：λ**A** = **A**λ
* 结合律：(λμ)**A** = λ(μ**A**)
* 分配律：(λ+μ)**A** = λ**A** + μ**A**；λ(**A** + **B**) = λ**A** + λ**B**

#### 三、矩阵与矩阵相乘
> 设 **A**=($a\_{ij}$) 是一个 $m\times s$ 矩阵， **B**=($b\_{ij}$) 是一个 $s\times n$ 矩阵，
> 那么规定矩阵 **A** 与矩阵 **B** 的乘积是一个 $m\times n$ 矩阵 **C**=($c\_{ij}$) ，其中
> 
> <div>
> $$c_{ij}=a_{i1}b_{1j}+a_{i2}b_{2j}+\cdots+a_{is}b_{sj}=\sum_{k=1}^s a_{ik}b_{kj},
> (i=1,2,\dots,m;j=1,2,\cdots,n),$$
> </div>
> 
> 并把此乘积记作
> 
> <div>
> $$\mathbf{C}=\mathbf{AB}$$
> </div>

如矩阵 **C** 为矩阵 **A** 与矩阵 **B** 的乘积，其中

<div>
$$
\mathbf{A}=\begin{bmatrix}
1 & 2 & 1 \\
3 & 1 & 2 \\
1 & 2 & 2 \\
3 & 1 & 1 \\
\end{bmatrix},\mathbf{B}=\begin{bmatrix}
1 & 2 \\
0 & 1 \\
2 & 3 \\
\end{bmatrix}
$$
</div>

可以看到矩阵 **A** 的列数刚好等于矩阵 **B** 的行数，根据定义矩阵 **A** 左乘矩阵 **B** ，即 **A** $\times$ **B** 有意义，
而矩阵 **B** 的列数不等于矩阵 **A** 的行数，所以矩阵 **A** 右乘矩阵 **B** ，即 **B** $\times$ **A** 无意义，
可以得出结论：矩阵与矩阵相乘不满足交换律，即 **AB**=**BA** 不一定成立。根据定义我们可以得出

<div>
$$
\mathbf{C}=\mathbf{AB}=\begin{bmatrix}
1\times 1+2\times 0+1\times 2 & 1\times 2+2\times 1 +1\times 3\\
3\times 1+1\times 0+2\times 2 & 3\times 2+1\times 1 +2\times 3\\
1\times 1+2\times 0+2\times 2 & 1\times 2+2\times 1 +2\times 3\\
3\times 1+1\times 0+1\times 2 & 3\times 2+1\times 1 +1\times 3
\end{bmatrix}
$$
</div>
<div>
$$=
\begin{bmatrix}
3 & 7\\
7 & 13\\
5 & 10\\
5 & 10
\end{bmatrix}
$$
</div>

矩阵与矩阵相乘虽不满足交换律，但仍满足以下规律（前提：两个矩阵相乘有意义）：

* 结合律：(**AB**)**C**=**A**(**BC**)；
* 数与矩阵相乘的交换律：λ(**AB**)=(λ**A**)**B**=**A**(λ**B**)
* 分配律：**A**(**B**+**C**)=**AB**+**AC**；(**B**+**C**)**A**=**BA**+**CA**

### Android 中的矩阵
android 的 graphic 包下有两个与矩阵运算直接相关的类： ColorMatrix 与 Matrix 。
简单的说， ColorMatrix 负责图像色彩特效的处理， Matrix 负责图像图形特效处理。

#### 一、ColorMatrix
android 中使用一个列矩阵来保存图片每个像素点的 RGBA 值

<div>
$$\begin{bmatrix}
R\\
G\\
B\\
A\\
1
\end{bmatrix}
$$
</div>

而对图片的色彩进行处理 android 是使用一个 $4 \times 5$ 的矩阵与图片每个像素由 RBGA 值表示的列矩阵进行运算，如

<div>
$$
\begin{bmatrix}
a & b & c & d & e\\
f & g & h & i & j\\
k & l & m & n & o\\
p & q & r & s & t\\
\end{bmatrix}
\times
\begin{bmatrix}
R\\
G\\
B\\
A\\
1
\end{bmatrix}=
$$
</div>

<div>
$$
\begin{bmatrix}
aR + bG + cB + dA + e\\
fR + gG + hB + iA + j\\
kR + lG + mB + nA + o\\
pR + qG + rB + sA + t
\end{bmatrix}
$$
</div>

再规定

<div>
$$
R_{1}=aR + bG + cB + dA + e\\
G_{1}=fR + gG + hB + iA + j\\
B_{1}=kR + lG + mB + nA + o\\
A_{1}=pR + qG + rB + sA + t\\
$$
</div>

这个新的列矩阵就能表示变化后像素的 RGBA 值

<div>
$$
\begin{bmatrix}
R_{1}\\
G_{1}\\
B_{1}\\
A_{1}\\
1
\end{bmatrix}
$$
</div>

显而易见，当我们使用一个 a=g=m=s=1 ,其余皆为 0 的矩阵去处理图片时必定不会有任何效果上的变化，我们可以称这样的矩阵为初始矩阵，
在它之上修改相关的值也很方便讲解。如图：

![初始状态](http://ww3.sinaimg.cn/mw690/006nezkigw1f0p2lfksmcj308m0fcab4.jpg)

![如果设置 RGBA 中 R 的系数为 3 ，则整个图片将偏红，如果设置 R 和 G 的系数为 3 ，即红色和绿色同比例增加其效果将偏黄](http://ww2.sinaimg.cn/mw690/006nezkigw1f0p2zxemhtj30ha0fc77a.jpg)

![若 R ， G ， B 的系数均为 3 ，则图片变亮，且系数越大图片越亮](http://ww4.sinaimg.cn/mw690/006nezkigw1f0p2zycipjj30hc0fcacz.jpg)

##### 具体实现
用 GridView 存放 $4\times 5$ 个 EditText ，每次点击 change 时获取 EditText 里的数值， 20 个值以数组的形式设置到一个 colorMatrix 中，
通过 `new ColorMatrixColorFilter(colorMatrix)` 构造出滤镜再设置给 paint ，使用设置了滤镜的 paint 进行绘画时就能出现相应的效
果了，具体代码如下：

```java
public class FmRGBAMatrix extends Fragment implements View.OnClickListener{

    private Button changeMatrix, resetMatrix;
    private GridView gridView;
    private ImageView imageView;
    private Bitmap picture;
    private float[] mMatrix;
    private List<EditText> mEts;
    private GridAdapter gridAdapter;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    	mMatrix = new float[20];
        mEts = new ArrayList<>();

        View view = inflater.inflate(R.layout.fm_rgba_matrix, container, false);

        gridView = (GridView) view.findViewById(R.id.group);
        imageView = (ImageView) view.findViewById(R.id.imageview);
        changeMatrix = (Button) view.findViewById(R.id.change);
        resetMatrix = (Button) view.findViewById(R.id.reset);
        picture = BitmapFactory.decodeResource(getResources(), R.drawable.cat);
        imageView.setImageBitmap(picture);
        gridView.setNumColumns(5);
        for (int i = 0; i < 20; i++) {
            EditText ed = new EditText(getContext());
            mEts.add(ed);
        }
        gridAdapter = new GridAdapter(mEts);
        gridView.setAdapter(gridAdapter);
        initMatrix();
        changeMatrix.setOnClickListener(this);
        resetMatrix.setOnClickListener(this);
        return view;
    }

    private void initMatrix() {
        for (int i = 0; i < 20; i++) {
            if (i % 6 == 0) {
                mEts.get(i).setText(String.valueOf(1));
            } else {
                mEts.get(i).setText(String.valueOf(0));
            }
        }
        gridAdapter.notifyDataSetChanged();
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()){
            case R.id.change:
                getMatrix();
                setImageMatrix();
                break;
            case R.id.reset:
                initMatrix();
                getMatrix();
                setImageMatrix();
        }
    }

    private void setImageMatrix() {
        Bitmap bmp = Bitmap.createBitmap(picture.getWidth(),picture.getHeight(), Bitmap.Config.ARGB_8888);
        Canvas canvas = new Canvas(bmp);
        ColorMatrix colorMatrix = new ColorMatrix();
        colorMatrix.set(mMatrix);
        Paint paint = new Paint();
        paint.setColorFilter(new ColorMatrixColorFilter(colorMatrix));
        canvas.drawBitmap(picture,0,0,paint);
        imageView.setImageBitmap(bmp);
    }

    private void getMatrix() {
        for (int i = 0; i < 20; i++){
            mMatrix[i] = Float.valueOf(mEts.get(i).getText().toString());
        }
    }
}
```
##### 效果叠加
假设有两个矩阵分别都能对图片产生效果，那么两个矩阵的乘法并得出新的矩阵可以用来表示叠加效果，如：

<div>
$$
\begin{bmatrix}
a_{1} & b_{1} & c_{1} & d_{1} & e_{1}\\
f_{1} & g_{1} & h_{1} & i_{1} & j_{1}\\
k_{1} & l_{1} & m_{1} & n_{1} & o_{1}\\
p_{1} & q_{1} & r_{1} & s_{1} & t_{1}\\
0 & 0 & 0 & 0 & 1
\end{bmatrix}\times \begin{bmatrix}
a_{2} & b_{2} & c_{2} & d_{2} & e_{2}\\
f_{2} & g_{2} & h_{2} & i_{2} & j_{2}\\
k_{2} & l_{2} & m_{2} & n_{2} & o_{2}\\
p_{2} & q_{2} & r_{2} & s_{2} & t_{2}\\
0 & 0 & 0 & 0 & 1
\end{bmatrix}
$$
</div>

查看源码，可以发现在 `ColorMatrix` 的中的 `setConcat` 方法就实现了两个矩阵 `matA` 与 `matB` 的相乘，
 `preConcat` 与 `postConcat` 只是调用 `setConcat` 方法。 `preConcat` 表示当前矩阵左乘 `prematrix` ， 
 `postConcat` 表示当前矩阵右乘 `postmatrix ` 。
```java
public void preConcat(ColorMatrix prematrix) {
    setConcat(this, prematrix);
}

public void postConcat(ColorMatrix postmatrix) {
    setConcat(postmatrix, this);
}
public void setConcat(ColorMatrix matA, ColorMatrix matB) {
    float[] tmp;
    if (matA == this || matB == this) {
        tmp = new float[20];
    } else {
        tmp = mArray;
    }

    final float[] a = matA.mArray;
    final float[] b = matB.mArray;
    int index = 0;
    for (int j = 0; j < 20; j += 5) {
        for (int i = 0; i < 4; i++) {
            tmp[index++] = a[j + 0] * b[i + 0] +  a[j + 1] * b[i + 5] +
                           a[j + 2] * b[i + 10] + a[j + 3] * b[i + 15];
        }
        tmp[index++] = a[j + 0] * b[4] +  a[j + 1] * b[9] +
                       a[j + 2] * b[14] + a[j + 3] * b[19] +
                       a[j + 4];
    }

    if (tmp != mArray) {
        System.arraycopy(tmp, 0, mArray, 0, 20);
    }
}
```

##### 例子
用三个 SeekBar 分别调用 ColorMatrix 内置的方法 `setRotate(int axis, float degree)` 、 `setSaturation(float sat)` 、 
`setScale(float rScale, float gScale, float bScale,float aScale)` 来控制图片的色调、饱和度和亮度。

* `setRotate(int axis, float degree)`：

第一个参数只能为 0 ， 1 ， 2 ，否则抛出运行时异常， 0 ， 1 ， 2 分别代表 R ， G ， B ；第二个参数是角度，在该方法内部中被转化为弧度进行计算。

* `setSaturation(float sat)`：

设置图片饱和度，参数为 0 时为灰度图像。

* `setScale(float rScale, float gScale, float bScale,float aScale)`:

四个参数分别为 RGBA 的系数，让 RGB 的系数同比例变化可表现出变亮或变暗的效果。

![初始状态](http://ww4.sinaimg.cn/mw690/006nezkigw1f0qs9ejo0xj308m0fcaat.jpg)

![调节饱和度](http://ww3.sinaimg.cn/mw690/006nezkigw1f0qs9gfhwlj308m0fc3z5.jpg)

![同时调节饱和度和亮度](http://ww3.sinaimg.cn/mw690/006nezkigw1f0qs9kg7auj308m0fcaaj.jpg)

核心代码：
```java
public static Bitmap handleEffect(Bitmap bm, float hue, float saturation, float lum) {
    Bitmap bmp = Bitmap.createBitmap(bm.getWidth(), bm.getHeight(), Bitmap.Config.ARGB_8888);
    Canvas canvas = new Canvas(bmp);
    Paint paint = new Paint();

    ColorMatrix hueMatrix = new ColorMatrix();
    hueMatrix.setRotate(0, hue);
    hueMatrix.setRotate(1, hue);
    hueMatrix.setRotate(2, hue);

    ColorMatrix saturationMatrix = new ColorMatrix();
    saturationMatrix.setSaturation(saturation);

    ColorMatrix lumMatrix = new ColorMatrix();
    lumMatrix.setScale(lum, lum, lum, 1);

    ColorMatrix colorMatrix = new ColorMatrix();
    colorMatrix.postConcat(hueMatrix);
    colorMatrix.postConcat(saturationMatrix);
    colorMatrix.postConcat(lumMatrix);

    paint.setColorFilter(new ColorMatrixColorFilter(colorMatrix));
    canvas.drawBitmap(bm, 0, 0, paint);
    return bmp;
}
```
界面代码：
```java
public class FmRGBASeekbar extends Fragment {

    private SeekBar seekbarHue, seekbarSaturation, seekbarLum;
    private float mHue, mSaturation = 1.0f, mLum = 1.0f;
    private Bitmap picture;
    private ImageView imageView;
    private int MID_VALUE = 50;
    private SeekBar.OnSeekBarChangeListener listener = new SeekBar.OnSeekBarChangeListener() {
        @Override
        public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
            switch (seekBar.getId()){
                case R.id.seekbarHue:
                    mHue = (progress - MID_VALUE) * 1.0F / MID_VALUE * 180;
                    break;
                case R.id.seekbarSaturation:
                    mSaturation = progress * 1.0F / MID_VALUE;
                    break;
                case R.id.seekbarLum:
                    mLum = progress * 1.0F / MID_VALUE;
                    break;
            }
            imageView.setImageBitmap(ImageUtil.handleEffect(picture,mHue,mSaturation,mLum));
        }

        @Override
        public void onStartTrackingTouch(SeekBar seekBar) {

        }

        @Override
        public void onStopTrackingTouch(SeekBar seekBar) {

        }
    };
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

        View view = inflater.inflate(R.layout.fm_rgba_seekbar, container, false);
        imageView = (ImageView) view.findViewById(R.id.imageview);
        seekbarHue = (SeekBar) view.findViewById(R.id.seekbarHue);
        seekbarSaturation = (SeekBar) view.findViewById(R.id.seekbarSaturation);
        seekbarLum = (SeekBar) view.findViewById(R.id.seekbarLum);

        seekbarHue.setMax(2 * MID_VALUE);
        seekbarSaturation.setMax(2 * MID_VALUE);
        seekbarLum.setMax(2 * MID_VALUE);
        seekbarHue.setProgress(MID_VALUE);
        seekbarSaturation.setProgress(MID_VALUE);
        seekbarLum.setProgress(MID_VALUE);

        seekbarHue.setOnSeekBarChangeListener(listener);
        seekbarSaturation.setOnSeekBarChangeListener(listener);
        seekbarLum.setOnSeekBarChangeListener(listener);

        picture = BitmapFactory.decodeResource(getResources(),R.drawable.cat);
        imageView.setImageBitmap(picture);

        return view;
    }


}
```
#### 二、Matrix
图片的每个像素点的位置信息同样可以用一个列矩阵来表示

<div>
$$
\begin{bmatrix}
X\\
Y\\
1
\end{bmatrix}
$$
</div>

对图片进行图形处理是用一个 $3 \times 3$ 的矩阵去左乘一个列矩阵

<div>
$$
\begin{bmatrix}
scaleX & skewX & translateX\\
skewY & scaleY & translateY\\
0 & 0 & 1
\end{bmatrix}\times \begin{bmatrix}
X_{0}\\
Y_{0}\\
1
\end{bmatrix}=
$$
</div>

<div>
$$
\begin{bmatrix}
X_{0}\times scaleX +Y_{0} \times skewX + translateX\\
X_{0}\times scaleY + Y_{0}\times skewY + translateY\\
1
\end{bmatrix}=
$$
</div>

<div>
$$
\begin{bmatrix}
X_{1}\\
Y_{1}\\
1
\end{bmatrix}
$$
</div>

根据矩阵乘法规则可知：当矩阵的 scaleX=scaleY=1 ， skewX=skewY=translateX=translateY=0 时图片不会有效果上的变化。

![初始状态](http://ww1.sinaimg.cn/mw690/006nezkigw1f0r69z0gvej308m0fcgmi.jpg)

* 平移

图片的所有点沿 X 轴方向移动 Δx ，沿 Y 轴方向移动 ΔY 就能实现整个图片的平移。

![](http://ww2.sinaimg.cn/mw690/006nezkigw1f0thi2pj2lj30au08uaa3.jpg)

如图点 $P_0(x_0,y_0)$ 移动到点 $P_1(x_0+\Delta x,y_1+\Delta y)$ 使用矩阵表示即为:

<div>
$$
\begin{bmatrix}
1 & 0 & \Delta x\\
0 & 1 & \Delta y\\
0 & 0 & 1
\end{bmatrix}\times \begin{bmatrix}
x_{0}\\
y_{0}\\
1
\end{bmatrix}
$$
</div>

使用 graphic 包下的 Matrix 类提供的 `setTranslate(float Δx, float Δy)` 方法就能很简便的配置 Matrix 实例。
```java
Matrix translateMatrix = new Matrix();
translateMatrix.setTranslate(100.0f, 100.0f);
```

![右移100px下移100px](http://ww4.sinaimg.cn/mw690/006nezkigw1f0r6a262ttj308m0fcdgp.jpg)

* 旋转

选取图片中的一点作为中心，图片所有点均绕中心点旋转到新的点就能实现整个图片的旋转。

![](http://ww2.sinaimg.cn/mw690/006nezkigw1f0thi43hg0j30a308tq30.jpg)

如图点 $P_0$ 绕原点旋转 θ° 到达 $P_1$ ，点 $P_0$ 坐标可以表示为 OP 与 X 轴正方向夹角之间的函数表达式：

<div>
$$
x_{0}=r_{0}\cos \alpha \\
y_{0} = r_{0}\sin \alpha \\
x_{1} = r_{0}\cos (\alpha + \theta)= r_{0}\cos \alpha \cos \theta - r_{0}\sin \alpha \sin \theta\\
=x_{0}\cos \theta - y_{0}\sin \theta\\
y_{1} = r_{0}\sin (\alpha + \theta)= r_{0}\sin \alpha \cos \theta + r_{0}\cos \alpha \sin \theta\\
= x_{0}\sin \theta + y_{0}\cos \theta
$$
</div>

使用矩阵表示即为：

<div>
$$
\begin{bmatrix}
\cos \theta & -\sin \theta & 0\\
\sin \theta & \cos \theta & 0\\
0 & 0 & 1
\end{bmatrix}\times \begin{bmatrix}
x_{0}\\
y_{0}\\
1
\end{bmatrix}
$$
</div>

`setRotate(float degrees, float px, float py)`:第一个参数表示选转的角度，后两个参数确定旋转中心。

```java
Matrix rotateMatrix = new Matrix();
rotateMatrix.setRotate(60.0f, picture.getWidth() / 2 + imageView.getPaddingLeft(),
    picture.getHeight() / 2 + imageView.getPaddingTop());
```

![绕图片中心顺时针旋转60°](http://ww1.sinaimg.cn/mw690/006nezkigw1f0r6a50d3gj308m0fcmy2.jpg)

* 缩放

单个的像素点没有缩放的概念，但图片所有像素点按照一定比例计算出新的坐标点，就能实现图片整体上的缩放。
如坐标点 $(x_0,y_0)$ 经过缩放后得到新的坐标点 $(x_1,y_1)$ 。

<div>
$$
x_{1}=k_{1}x_{0}\\
y_{1}=k_{2}y_{0}
$$
</div>

使用矩阵表示：

<div>
$$
\begin{bmatrix}
k_{1} & 0 & 0\\
0 & k_{2} & 0\\
0 & 0 & 1
\end{bmatrix}\times \begin{bmatrix}
x_{0}\\
y_{0}\\
1
\end{bmatrix}
$$
</div>

`setScale(float sx, float sy)`：参数分别确定 x 轴和 y 轴方向上的缩放比例。
```java
Matrix scaleMatrix = new Matrix();
scaleMatrix.setScale(0.5f, 0.5f);
```

![x轴y轴方向均缩放为原来的一半](http://ww3.sinaimg.cn/mw690/006nezkigw1f0r6a5qfrwj308m0fcmxo.jpg)

* 错切

垂直错切(或水平错切)：像素点保持 X 轴坐标(或 y 轴坐标)不变， y 轴坐标(或 x 轴坐标)按一定比例发生改变，且改变的大小与 x 轴坐标(或 y 轴坐标)成正比。

![垂直错切](http://ww2.sinaimg.cn/mw690/006nezkigw1f0tlhdoyk2j30ac08d0su.jpg)

如图点 $P_0$ 保持 x 轴坐标不变移动到 $P_1$ ，其中有：

<div>
$$
x_{1} = x_{0} + k_{1}y_{0}\\
y_{1} = y_{0} + k_{2}x_{0}
$$
</div>

使用矩阵表示：

<div>
$$
\begin{bmatrix}
1 & k_{1} & 0 \\
k_{2} & 1 & 0 \\
0 & 0 & 1
\end{bmatrix}\times \begin{bmatrix}
x_{0}\\
y_{0}\\
1
\end{bmatrix}
$$
</div>

$k_1 = 0,k_2\neq 0$ 为垂直错切； $k_1\neq 0,k_2 = 0$ 为水平错切；当然 $k_1,k_2$ 可以均不为 0 ，此时水平、垂直方向同时错切。

`setSkew(float kx, float ky)`:参数分别确定 x 轴坐标变化大小与原始 y 轴坐标的比例， y 轴坐标变化大小与原始 x 轴坐标的比例。

![y轴方向的错切变换](http://ww2.sinaimg.cn/mw690/006nezkigw1f0r6a7axooj308m0fcaay.jpg)

```java
public class FmMatrix extends Fragment implements View.OnClickListener {

    private Button translateBtn, skewBtn, rotateBtn, scaleBtn;
    private ImageView imageView;
    private Bitmap picture;
    private Bitmap product;
    private Canvas mCanvas;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

        View view = inflater.inflate(R.layout.fm_matrix, container, false);
        translateBtn = (Button) view.findViewById(R.id.translate);
        skewBtn = (Button) view.findViewById(R.id.skew);
        rotateBtn = (Button) view.findViewById(R.id.rotate);
        scaleBtn = (Button) view.findViewById(R.id.scale);
        imageView = (ImageView) view.findViewById(R.id.imageview);

        picture = BitmapFactory.decodeResource(getResources(), R.drawable.cat);
        product = Bitmap.createBitmap(picture.getWidth() * 2, picture.getHeight() * 2,
                Bitmap.Config.ARGB_8888);
        imageView.setImageBitmap(picture);
        mCanvas = new Canvas(product);

        translateBtn.setOnClickListener(this);
        skewBtn.setOnClickListener(this);
        rotateBtn.setOnClickListener(this);
        scaleBtn.setOnClickListener(this);

        return view;
    }

    @Override
    public void onClick(View v) {
        mCanvas.drawColor(Color.WHITE);
        switch (v.getId()) {
            case R.id.translate:
                Matrix translateMatrix = new Matrix();
                translateMatrix.setTranslate(100.0f, 100.0f);
                mCanvas.drawBitmap(picture, translateMatrix, null);
                break;
            case R.id.skew:
                Matrix skewMatrix = new Matrix();
                skewMatrix.setSkew(0.0f, 0.8f);
                mCanvas.drawBitmap(picture, skewMatrix, null);
                break;
            case R.id.rotate:
                Matrix rotateMatrix = new Matrix();
                rotateMatrix.setRotate(60.0f, picture.getWidth() / 2 + imageView.getPaddingLeft(),
                        picture.getHeight() / 2 + imageView.getPaddingTop());
                mCanvas.drawBitmap(picture, rotateMatrix, null);
                break;
            case R.id.scale:
                Matrix scaleMatrix = new Matrix();
                scaleMatrix.setScale(0.5f, 0.5f);
                mCanvas.drawBitmap(picture, scaleMatrix, null);
                break;
        }
        imageView.setImageBitmap(product);
    }
}
```
### 参考

[《Android 群英传》第六章](http://baike.baidu.com/link?url=bzzHOYD7wyh_jrzymzlSOWTYXmipdudcZ6wH8mty0mIO0XcGhknUT9wJlLrE0RjxUEEblS99SBwbVBCHxULY7Xj8VyxDcGLVVpU7l7TKPb8BIuAzJL0iqa1bw9Y-l-7F)

[工程数学线性代数第五版](http://baike.baidu.com/link?url=dTqZyGD-SSx9tOsdoLJvWu1Bf0SEEhAdfoli8F0FqxfWgnZUDenOwm4zsG9q1j059z8tDKY1MHnGRprbqkByiq)

[Android Matrix](http://www.cnblogs.com/qiengo/archive/2012/06/30/2570874.html#code)
