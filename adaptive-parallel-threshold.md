# Adaptive Parallel Threshold



```csharp
public static double VariableThresholdingLocalPropertiesParallel(Bitmap image, double a, double b)
{
    int w = image.Width;
    int h = image.Height;

    BitmapData image_data = image.LockBits(
        new Rectangle(0, 0, w, h),
        ImageLockMode.ReadOnly,
        PixelFormat.Format24bppRgb);

    int bytes = image_data.Stride * image_data.Height;
    byte[] buffer = new byte[bytes];

    Marshal.Copy(image_data.Scan0, buffer, 0, bytes);
    image.UnlockBits(image_data);

    // Get global mean - this works only for grayscale images
    double mg = 0;
    for (int i = 0; i < bytes; i += 3)
    {
        mg += buffer[i];
    }
    mg /= (w * h);

    // 병렬 처리 준비: 임계값의 누적 합과 픽셀 수
    double cumulativeThreshold = 0;
    int count = 0;
    object lockObject = new object(); // 동기화 객체

    Parallel.For(1, w - 1, x =>
    {
        double localCumulativeThreshold = 0; // 각 스레드에서 임계값 누적 합산
        int localCount = 0; // 각 스레드에서 처리한 픽셀 수

        for (int y = 1; y < h - 1; y++)
        {
            int position = x * 3 + y * image_data.Stride;
            double mean = 0;
            double std = 0;

            // 3x3 주변 픽셀의 평균과 표준 편차 계산
            for (int i = -1; i <= 1; i++)
            {
                for (int j = -1; j <= 1; j++)
                {
                    int nposition = position + i * 3 + j * image_data.Stride;
                    byte pixelValue = buffer[nposition];
                    mean += pixelValue;
                    std += pixelValue * pixelValue;
                }
            }

            mean /= 9.0;
            std = Math.Sqrt(std / 9.0 - mean * mean);

            // 임계값 계산
            double threshold = a * std + b * mg;
            localCumulativeThreshold += threshold;
            localCount++;
        }

        // 누적된 결과를 메인 변수에 병합
        lock (lockObject)
        {
            cumulativeThreshold += localCumulativeThreshold;
            count += localCount;
        }
    });

    // 모든 픽셀의 평균 임계값을 반환
    return cumulativeThreshold / count;
}
```
