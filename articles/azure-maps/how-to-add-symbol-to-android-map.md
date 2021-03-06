---
title: 向 Android maps 添加符号层 |Microsoft Azure 映射
description: 了解如何向地图添加标记。 查看一个示例，该示例使用 Azure Maps Android SDK 添加一个符号层，其中包含来自数据源的基于点的数据。
author: anastasia-ms
ms.author: v-stharr
ms.date: 04/26/2019
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 10969e20cd7ae71cade230f6643a27d5d940ceaa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91311268"
---
# <a name="add-a-symbol-layer-to-a-map-using-azure-maps-android-sdk"></a>使用 Azure Maps 向地图添加符号层 Android SDK

本文说明如何使用 Azure Maps Android SDK 将数据源中的点数据呈现为地图上的符号层。

## <a name="prerequisites"></a>必备条件

若要完全按照本文中的步骤进行操作，需要安装 [Azure Maps Android SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) 来加载地图。

## <a name="add-a-symbol-layer"></a>添加符号层

若要使用符号层在地图上添加标记，请遵循以下步骤：

1. 编辑**res**  >  **布局**  >  **activity_main.xml**使其类似于以下 XML：
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_centerLat="47.64"
            app:mapcontrol_centerLng="-122.33"
            app:mapcontrol_zoom="12"
            />

    </FrameLayout>
    ```

2. 将以下代码片段复制到类的 **onCreate ( # B1 ** 方法 `MainActivity.java` 。

    ```Java
    mapControl.onReady(map -> {
    
        //Create a data source and add it to the map.
        DataSource dataSource = new DataSource();
        map.sources.add(dataSource);
    
        //Create a point feature and add it to the data source.
        dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
    
        //Add a custom image icon to the map resources.
        map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
    
        //Create a symbol layer and add it to the map.
        map.layers.add(new SymbolLayer(dataSource,
            iconImage("my-icon")));
        });
    
    ```
    
    上面的代码段首先使用 **onReady ( # B1 ** 回调方法获取 Azure Maps 映射控件实例。 然后，它使用 **DataSource** 类创建数据源对象并将其添加到地图中。 然后，它将包含点几何图形的功能添加到该 **功能** 中。 然后，将红色标记图像设置为符号的图标。 **符号层**使用文本或图标将基于点的数据作为地图上的符号在数据源中进行包装。 然后创建一个符号层，然后将数据源传递给它以呈现，然后将其添加到地图的层中。
    
    添加上述代码片段后，应如下 `MainActivity.java` 所示：
    
    ```Java
    package com.example.myapplication;
    
    import android.app.Activity;
    import android.os.Bundle;
    import com.mapbox.geojson.Feature;
    import com.mapbox.geojson.Point;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;
    import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
    public class MainActivity extends AppCompatActivity {
        
        static{
                AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
            }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {
    
                //Create a data source and add it to the map.
                DataSource dataSource = new DataSource();
                map.sources.add(dataSource);
            
                //Create a point feature and add it to the data source.
                dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
            
                //Add a custom image icon to the map resources.
                map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            
                //Create a symbol layer and add it to the map.
                map.layers.add(new SymbolLayer(dataSource,
                    iconImage("my-icon")));
            });
        }
    
        @Override
        public void onStart() {
            super.onStart();
            mapControl.onStart();
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }
    ```
    
此时，如果你运行应用程序，你应该会在地图上看到一个标记，如下所示：

<center>

![Android 地图图钉](./media/how-to-add-symbol-to-android-map/android-map-pin.png)</center>

> [!TIP]
> 默认情况下，符号层通过隐藏重叠的符号来优化符号的呈现。 放大时，隐藏的符号将变为可见。 若要禁用此功能并始终呈现所有符号，请将 `iconAllowOverlap` 选项设置为 `true` 。

## <a name="next-steps"></a>后续步骤

若要将更多内容添加到地图，请参阅：

> [!div class="nextstepaction"]
> [在 Android 地图中添加形状](https://docs.microsoft.com/azure/azure-maps/how-to-add-shapes-to-android-map)

> [!div class="nextstepaction"]
> [显示功能信息](display-feature-information-android.md)