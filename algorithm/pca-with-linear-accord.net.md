---
description: Implements principal component analysis(PCA) algorithm
---

# PCA with linear Accord.Net

### PCA Summary

주성분 분석은 차원 축소를 하기 위해 사용하는 알고리즘 기법이에요.

예시로 설명하면 x,y,z 의 3차원 좌표를 가지고 있는 리스트가 있다고 할 때, 이를 분석해서 경향성을 수치화해 2차원 좌표로 나타내게끔 하는 기법이에요.

실제 구현 예시 코드는 아래와 같아요.

### Example

{% code overflow="wrap" lineNumbers="true" %}
```csharp
public static List<double> GetPCADataErrorValue(List<double[]> coordinates)
{
	var originalData = coordinates.DeepClone();
	
	var data = coordinates.ToArray();
	var centroid = Measures.Mean(data, dimension: 0);
	
	PrincipalComponentAnalysis pca = new PrincipalComponentAnalysis()
	{
		Method = PrincipalComponentMethod.CovarianceMatrix,
		Means = centroid,
	};
	
	var adjustData = AdjustData(data, centroid);
	var covariance = Measures.Covariance(adjustData);
	pca.Learn(covariance);
	
	var transform = pca.Transform(originalData.ToArray());
	
	double maxDistance = 0;
	List<double> distanceList = new List<double>();
	// 각 점에서 주어진 선에 대한 거리 계산
	for (int i = 0; i < data.Length; ++i)
	{
		var distance = Math.Sqrt(Math.Pow(transform[i][1], 2) + Math.Pow(transform[i][2],2));
		distanceList.Add(distance);
		maxDistance = Math.Max(maxDistance, distance); 
	}
}

private static double[][] AdjustData(double[][] data, double[] centroid)
{
        for (int i = 0; i < data.Length; i++)
            for (int j = 0; j < data[i].Length; j++)
                data[i][j] -= centroid[j];

        return data;
}
```
{% endcode %}
