---
layout:     post
title:      "Network Dataset"
subtitle:   " \"GIS\""
date:       2017-01-21 10:41:00
author:     "PandaL33"
header-img: "img/post-bg-2015.jpg"
tags:
    - GIS
---
## 前言

&emsp;&emsp;网络数据集（Network Dataset)由Feature要素创建，可以构建无向网络。创建的网络能够用来表现复杂场景，包括Multimodal交通网络，同样也可以包含多个网络属性以模拟网络限制条件和层次结构。

&emsp;&emsp;网络数据集创建网络的数据源存储于Personal Geodatabase或者shapefile，其中Personal Geodatabase可以存储很多数据源，因此可以构建Multimodal Network，而基于Shapefile文件创建的Network Dataset，因为不能够支持多种Edge 类型，所以不能用于创建Multimodal Network。

## 网络数据集的相关元素

### 1、Connectivity

&emsp;&emsp;ArcCatlog中创建Network Dataset,需要设置网络的Connectivity（连通性），连通性有下面3中模式：
1. Connecting Edges within a Connectivity Group

>>  可以设置“Endpoint Connectivity”，也可以设置“Any Vertex Connectivity”。第一种方式中，边和边只能在终点处相交，第二种方式则可以在边的任意位置相交。
2. Connecting Edges through Junctions across Connectivity Group

>> 能够将不同Connectivity Group中的Edge通过被不同Connectivity Group共享的Junctions连接。
3. Elevation Fields

>> 主要用于Network Dataset中检查Line Endpoints的Connectivity。每一个Edge Feature具备两个字段用来描述每一个端点的高程。

### 2、Network Attribute

&emsp;&emsp;Network Attribute主要用于设定网络的流通属性，属性有：

- Name：名称
- Usage Type：usage notes类型
- Unit：Centimeter，Meter等等
- Data Types：Boolean，Integer，Float，Double
- Use by Default：设置为默认
- Cost：例如走过某段路需要花费的时间
- Descriptors：对某条道路的描述信息，例如道路速度的限制，有多少个红绿灯等。
- Restrictions：例如某条线是禁行，或者是单向的
- Hierarchy：例如道路的分级

### 3、Evaluators

&emsp;&emsp;Evaluators用来从Network Source中获取网络属性的值。四种Evaluators为：
- Field Evaluator：利用属性字段的值；
- Field Expression Evaluator：利用属性字段构建计算表达式；
- Constant Evaluators：赋予常数；
- VBscript Evaluators：通过执行VBScript代码，主要用于赋予复杂的属性值每个Junction Source和Turn Source需要一个Evaluator，而每个Edge Source需要两个-Edge的每个方向都需要一个Evaluator。

### 4、Turns

&emsp;&emsp;Turn是通过TurnFeatureClass(转向要素集)转变而来的，这些TurnFeatureClass都是Polyline FeatureClass。TurnFeatureClass必须是与其他Network要素位于同一个Feature Dataset中，具备相同的空间参考，不参与Connectivity Groups，也不具备Elevation信息。Turn至少具备两条Edge，至多 20条Edge。

## 网络分析

## 1、无向网络的创建

&emsp;&emsp;ArcCatlog中创建Network Dataset步骤：
1. 打开网络数据集的向导，定义几何网络的名称
2. 选择参与网络数据集的要素类
3. 设置是否使用转向数据集
4. 连通性设置
5. 是否使用高程字段模拟连通性
6. 设置权重
7. 是否建立行驶方向
7. 建立Network Dataset
 数据源：
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/network-dataset/network-dataset-1.jpeg)
### 创建网络数据集的实现：
```
/// <summary> 
///  创建无向网络 
/// </summary> 
/// <param name="_pWsName">个人数据库的路径</param> 
/// <param name="_pDatasetName">要素数据集的路径</param> 
/// <param name="_pNetName">建立网络的名称</param> 
/// <param name="_pFtName">参与网络的要素类</param>  
public void CreateNetworkDataset(string _pWsName, string _pDatasetName, string _pNetName, string _pFtName)
{
    IDENetworkDataset pDENetworkDataset = new DENetworkDatasetClass();
    pDENetworkDataset.Buildable = true;
    IWorkspace pWs = GetWorkspace(_pWsName);
    IFeatureWorkspace pFtWs = pWs as IFeatureWorkspace;
    IFeatureDataset pFtDataset = pFtWs.OpenFeatureDataset(_pDatasetName);
    //定义空间参考
    IDEGeoDataset pDEGeoDataset = (IDEGeoDataset)pDENetworkDataset;
    IGeoDataset pGeoDataset = pFtDataset as IGeoDataset;
    pDEGeoDataset.Extent = pGeoDataset.Extent;
    pDEGeoDataset.SpatialReference = pGeoDataset.SpatialReference;
    //网络数据集的名称 
    IDataElement pDataElement = (IDataElement)pDENetworkDataset;
    pDataElement.Name = _pNetName;
    //参加建立网络数据集的要素类 
    INetworkSource pEdgeNetworkSource = new EdgeFeatureSourceClass();
    pEdgeNetworkSource.Name = _pFtName;
    pEdgeNetworkSource.ElementType = esriNetworkElementType.esriNETEdge;
    //要素类的连通性 
    IEdgeFeatureSource pEdgeFeatureSource = 
    (IEdgeFeatureSource)pEdgeNetworkSource;
    pEdgeFeatureSource.UsesSubtypes = false;
    pEdgeFeatureSource.ClassConnectivityGroup = 1;
    pEdgeFeatureSource.ClassConnectivityPolicy = esriNetworkEdgeConnectivityPolicy.esriNECPEndVertex;
    //不用转弯数据 
    pDENetworkDataset.SupportsTurns = false;
    IArray pSourceArray = new ArrayClass();
    pSourceArray.Add(pEdgeNetworkSource);
    pDENetworkDataset.Sources = pSourceArray;
    //网络数据集的属性设置 
    IArray pAttributeArray = new ArrayClass();
    IEvaluatedNetworkAttribute pEvalNetAttr;
    INetworkAttribute2 pNetAttr2;
    INetworkFieldEvaluator pNetFieldEval;
    INetworkConstantEvaluator pNetConstEval;
    pEvalNetAttr = new EvaluatedNetworkAttributeClass();
    pNetAttr2 = (INetworkAttribute2)pEvalNetAttr;
    pNetAttr2.Name = "Meters";
    pNetAttr2.UsageType = esriNetworkAttributeUsageType.esriNAUTCost;
    pNetAttr2.DataType = esriNetworkAttributeDataType.esriNADTDouble;
    pNetAttr2.Units = esriNetworkAttributeUnits.esriNAUMeters;
    pNetAttr2.UseByDefault = false;
    pNetFieldEval = new NetworkFieldEvaluatorClass();
    pNetFieldEval.SetExpression("[Shape_Length]", "");
    //方向设置 
    pEvalNetAttr.set_Evaluator(pEdgeNetworkSource,
    esriNetworkEdgeDirection.esriNEDAlongDigitized,
    (INetworkEvaluator)pNetFieldEval);
    pEvalNetAttr.set_Evaluator(pEdgeNetworkSource,
    esriNetworkEdgeDirection.esriNEDAgainstDigitized,
    (INetworkEvaluator)pNetFieldEval);
    pNetConstEval = new NetworkConstantEvaluatorClass();
    pNetConstEval.ConstantValue = 0;
    pEvalNetAttr.set_DefaultEvaluator(esriNetworkElementType.esriNETEdge,
    (INetworkEvaluator)pNetConstEval);
    pEvalNetAttr.set_DefaultEvaluator(esriNetworkElementType.esriNETJunction,
    (INetworkEvaluator)pNetConstEval);
    pEvalNetAttr.set_DefaultEvaluator(esriNetworkElementType.esriNETTurn,
    (INetworkEvaluator)pNetConstEval);
    //一个网络数据集可以有多个属性
    pAttributeArray.Add(pEvalNetAttr);
    pDENetworkDataset.Attributes = pAttributeArray;
    // 创建网络数据集
    IDENetworkDataset2 _pDENetDataset = pDENetworkDataset as IDENetworkDataset2;
    IFeatureDatasetExtensionContainer pFeatureDatasetExtensionContainer =
    (IFeatureDatasetExtensionContainer)pFtDataset;
    IFeatureDatasetExtension pFeatureDatasetExtension =
    pFeatureDatasetExtensionContainer.FindExtension(
    esriDatasetType.esriDTNetworkDataset);
    IDatasetContainer2 pDatasetContainer2 =
    (IDatasetContainer2)pFeatureDatasetExtension;
    IDEDataset pDENetDataset = (IDEDataset)_pDENetDataset;
    INetworkDataset pNetworkDataset =
    (INetworkDataset)pDatasetContainer2.CreateDataset(pDENetDataset);
    //建立网络 
    INetworkBuild pNetworkBuild = (INetworkBuild)pNetworkDataset;
    pNetworkBuild.BuildNetwork(pGeoDataset.Extent);
}
```
创建网络数据集后数据变为：
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/network-dataset/network-dataset-2.jpeg)

2、最短路径分析

&emsp;&emsp;无向网络分析中需要用到两个主要接口：INAContext和INASolver。INAContext主要用来获取网络分析的对象，这些对象包含NetworkDataset、NASolver、NAClasses、NATraversalResult、 NALocator以及任何实现INAAgent接口的对象；INASolver主要用来创建NAContext和NALayer对象来执行实际的网络分析。

INAContext的数据成员为：
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/network-dataset/network-dataset-3.jpeg)

INASolver的数据成员为：
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/network-dataset/network-dataset-4.jpeg)

最短路径分析的实现：
```
1、获取网络数据集
/// <summary> 
/// 获取网络数据集 
/// </summary> 
/// <param name="pFeatureDataset">要素集</param> 
/// <param name="_pDatasetName">数据集名称</param> 
/// <param name="_pNetDatasetName">网络数据集名称</param> 
/// <returns>网络数据集</returns> 
public INetworkDataset GetNetDataset(IFeatureDataset pFeatureDataset, string _pDatasetName, string _pNetDatasetName)
{
    IFeatureDatasetExtensionContainer pFeatureDatasetExtensionContainer =
    pFeatureDataset as IFeatureDatasetExtensionContainer; 
    IFeatureDatasetExtension pFeatureDatasetExtension =
    pFeatureDatasetExtensionContainer.FindExtension(
    esriDatasetType.esriDTNetworkDataset);
    IDatasetContainer3 pDatasetContainer = pFeatureDatasetExtension as
    IDatasetContainer3; 
    IDataset pNetWorkDataset =
    pDatasetContainer.get_DatasetByName(
    esriDatasetType.esriDTNetworkDataset, _pNetDatasetName);
    return pNetWorkDataset as INetworkDataset;
}
```
2、创建网络分析上下文
```
/// <summary>
/// 创建网络分析上下文
/// </summary>
/// <param name="networkDataset">网络数据集</param>
/// <returns>网络分析上下文</returns>
public INAContext CreateNAContext(INetworkDataset networkDataset)
{
    IDatasetComponent datasetComponent = networkDataset as IDatasetComponent;
    IDENetworkDataset pDENetworkDataset = datasetComponent.DataElement as
    IDENetworkDataset;
    INASolver pNASolver = new NARouteSolverClass();
    INAContextEdit pNAContextEdit = pNASolver.CreateContext(pDENetworkDataset,
    pNASolver.Name) as INAContextEdit;
    pNAContextEdit.Bind(networkDataset, new GPMessagesClass());
    return pNAContextEdit as INAContext;
}
```
3、添加网络分析图层
```
/// <summary>
/// 添加网络分析图层
/// </summary>
/// <param name="pNaContext">网络分析上下文</param>
/// <param name="Mainmap">Map控件</param>
public void AddNALayer(INAContext pNaContext,AxMapControl Mainmap)
{
    INALayer naLayer = pNaContext.Solver.CreateLayer(pNaContext);
    ILayer pLayer = naLayer as ILayer;
    pLayer.Name = pNaContext.Solver.DisplayName;
    MainMap.AddLayer(pLayer, 0);
}
```
4、设置Stops要素类
```
/// <summary> 
///  设置Stops要素类
/// </summary> 
/// <param name="_pNaContext">网络分析上下文</param> 
/// <param name="_pFtClass">Stops的要素类</param> 
/// <param name="_pPointC">路径点集合</param> 
/// <param name="_pDist">捕捉距离 </param> 
public void NAStops(INAContext _pNaContext, IFeatureClass _pFtClass, IPointCollection _pPointC, double _pDist)
{
    INALocator pNAlocator = _pNaContext.Locator;
    for (int i = 0; i < _pPointC.PointCount; i++)
    {
        IFeature pFt = _pFtClass.CreateFeature();
        IRowSubtypes pRowSubtypes = pFt as IRowSubtypes;
        pRowSubtypes.InitDefaultValues();
        pFt.Shape = _pPointC.get_Point(i) as IGeometry;
        IPoint pPoint = null;
        INALocation pNalocation = null;
        pNAlocator.QueryLocationByPoint(_pPointC.get_Point(i), ref pNalocation,
        ref pPoint, ref _pDist);
        INALocationObject pNAobject = pFt as INALocationObject;
        pNAobject.NALocation = pNalocation;
        int pNameFieldIndex = _pFtClass.FindField("Name");
        pFt.set_Value(pNameFieldIndex, pPoint.X.ToString() + " ， "
        + pPoint.Y.ToString());
        int pStatusFieldIndex = _pFtClass.FindField("Status");
        pFt.set_Value(pStatusFieldIndex,
        esriNAObjectStatus.esriNAObjectStatusOK);
        int pSequenceFieldIndex = _pFtClass.FindField("Sequence");
        pFt.set_Value(_pFtClass.FindField("Sequence"),
        ((ITable)_pFtClass).RowCount(null));
        pFt.Store();
    }
}
```
5、获取分析信息
```
 IGPMessages gpMessages = new GPMessagesClass();
 bool pBool = pNaContext.Solver.Solve(pNaContext, gpMessages, null);
```
实现结果：
![alt](https://raw.githubusercontent.com/PandaL33/PandaL33.github.io/master/img/in-post/network-dataset/network-dataset-5.jpeg)