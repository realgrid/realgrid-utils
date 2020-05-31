# 필드와 컬럼 일괄 생성

여러 유용한 함수를 [realgrid2-utils.js](https://github.com/realgrid/realgrid2-utils) 파일로 제공합니다. 사용자는 자유롭게 수정하거나 확장해 사용할 수 있습니다.
[RealGrid2](http://docs.realgrid.com)를 활용하는 방법의 한 예로 제공하는 유틸 함수를 모아 놓았습니다.

그 중 간단한 컬럼정보 만으로 DataProvider의 Field와 GridView의 Column을 동시에 생성하게 하는 함수인 setFieldsNColumns() 사용 예제 입니다.

```js
	var dataProvider, gridView;
	var rows;
		
	//dataType이 text가 아닌경우 tag속성에 지정
	var columns_01 = [{
	    "name": "Country",
	    "type": "data",
	    "width": "100",
	    "header": {
	        "text": "Country"
	    }
	}, {
	    "name": "Quantity",
	    "type": "data",
	    "width": "100",
			"tag": {"dataType": "number"},    //dataType이 text가 아닌경우 tag속성에 지정
	    "header": {
	        "text": "Quantity"
	    },
	    "footer": {
	        "expression": "sum",
	    },
	    "numberFormat": "#,##0"
	}, {
	    "name": "OrderDate",
	    "type": "data",
	    "width": "100",
			"tag": {"dataType": "datetime"}, //dataType이 text가 아닌경우 tag속성에 지정
	    "header": {
	        "text": "OrderDate"
	    }
  }]

	dataProvider = new RealGrid.LocalDataProvider();
  gridView = new RealGrid.GridView("realgrid");
  gridView.setDataSource(dataProvider);
  
  setFieldsNColumns(dataProvider, gridView, columns_01);
  
  //필드,컬럼 동적 생성
  function setFieldsNColumns(provider, grid, columnInfo) {
      var fields = [];

      for (var key in columnInfo) {
          var col = columnInfo[key];

   	 if (!col.fieldName) col.fieldName = col.name;
				if (col && (!col.items)) {
					//field 구성
					var f = {};
					f.fieldName = col.name;
					if (col.tag && col.tag.dataType) f.dataType = col.tag.dataType;

					fields
					fields.push(f);
				};
      };

      provider.setFields(fields);
      grid.setColumns(columnInfo);
 	 };
```

columnInfo에 정의한 정보를 바탕으로 setFieldsNColumns 함수가 필드와 컬럼을 생성해 줍니다.
