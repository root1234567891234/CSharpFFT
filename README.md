# CSharpFFT
```c# 8
var d = new Mat(filepath, ImreadModes.Grayscale);
d.ConvertTo(d, MatType.CV_32F);
var com = new Mat();
var p = new Mat();
int m = Cv2.GetOptimalDFTSize(d.Rows);
int n = Cv2.GetOptimalDFTSize(d.Cols);
Cv2.CopyMakeBorder(d, p, 0, m - d.Rows, 0, n - d.Cols, BorderTypes.Constant, Scalar.All(0));
Mat[] planes = new Mat[2];
planes[0] = new Mat<float>(p);
planes[1] = Mat.Zeros(p.Size(), MatType.CV_32F);
Cv2.Merge(planes, com);
Cv2.Dft(com, com);
Cv2.Split(com, out planes);
Cv2.Magnitude(planes[0], planes[1], planes[0]);
Mat magl = planes[0];
magl += Scalar.All(1);
Cv2.Log(magl, magl);
magl = magl.Clone(new Rect(0, 0, magl.Cols & -2, magl.Rows & -2));
int cx = magl.Cols / 2;
int cy = magl.Rows / 2;
Mat q0 = new Mat(magl, new Rect(0, 0, cx, cy));
Mat q1 = new Mat(magl, new Rect(cx, 0, cx, cy));
Mat q2 = new Mat(magl, new Rect(0, cy, cx, cy));
Mat q3 = new Mat(magl, new Rect(cx, cy, cx, cy));
Mat tmp = new Mat();
q0.CopyTo(tmp);
q3.CopyTo(q0);
tmp.CopyTo(q3);
q1.CopyTo(tmp);
q2.CopyTo(q1);
tmp.CopyTo(q2);
Cv2.Normalize(magl, magl, 0.0, 255, NormTypes.MinMax);
magl.SaveImage("fft.jpg");
```
