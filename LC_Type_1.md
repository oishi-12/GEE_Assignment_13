var roi = 
    /* color: #98ff00 */
    /* shown: false */
    /* displayProperties: [
      {
        "type": "rectangle"
      }
    ] */
    ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Polygon(
                [[[91.7378193203886, 22.55514687845212],
                  [91.7378193203886, 22.4657063744662],
                  [91.89574778718547, 22.4657063744662],
                  [91.89574778718547, 22.55514687845212]]], null, false),
            {
              "system:index": "0"
            })]);

var imgC = ee.ImageCollection("MODIS/061/MCD12Q1")
          .first()
          .clip(roi)
          .select("LC_Type1")
          
var vizParam = {
  min: 0,
  max: 17};
  
Map.addLayer(imgC, vizParam, "img_collection")
print(imgC);

var areaImage = ee.Image.pixelArea().addBands(imgC)
print(areaImage)

var classArea = areaImage.reduceRegion({
  geometry: roi.geometry(),
  scale: 500,
  reducer: ee.Reducer.sum().group({
    groupField:1,
    groupName: "Class"
  })
})
print("Class-wise Area:", classArea)
