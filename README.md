# web-mercator-wsut-killer
A project to track tasks associated with transitioning web apps to web mercator base maps and away from the old WSUT services.

## The happy path to using AGRC web mercator base maps

- [ ] bower install `agrc-layer-selector`
  - [ ] git ignore `layer-selector`
- [ ] add `layer-selector` to the require packages
- [ ] update the build profile `userConfig.packages` to get templates inlined
- [ ] update main css file to include `layer-selectr/resources/LayerSelector.css` 
- [ ] Update a secret config to have your quad words.
```js
    xhr(require.baseUrl + 'secrets.json', {
        handleAs: 'json',
        sync: true
    }).then(function (secrets) {
        window.AGRC.quadWord = secrets.quadWord;
    }, function () {
        throw 'Error getting secrets!';
    });
```
- [ ] Find reference to the `agrc/widgets/map/BaseMapSelector`
  - [ ] Replace them with `layer-selector`
```js
        new BaseMapSelector({
            map: this.map,
            quadWord: config.quadWord,
            baseLayers: ['Hybrid', 'Lite', 'Terrain', 'Topo', 'Color IR']
        });
```
- [ ] Update extent on `agrc/widgets/map/BaseMap`
```js
        new BaseMap(mapDiv, {
            useDefaultBaseMap: false,
            extent: new Extent({
                xmax: -12010849.397533866,
                xmin: -12898741.918094235,
                ymax: 5224652.298632992,
                ymin: 4422369.249751998,
                spatialReference: {
                    wkid: 3857
                }
            })
        });
```
- [ ] Update `agrc.widgets` to version >= `4.0.0`
- [ ] Find all references to `agrc/widgets/locate/FindAddress`
  - [ ] Update `zoomLevel` to `17` to handle new tile schema
- [ ] Find all refereces to `agrc/widgets/locate/MagicZoom`
  - [ ] Add `apiKey` ctor property
  - [ ] Add `wkid` ctor property
  - [ ] Add `searchLayer` if not pointing at gnis layer
  - [ ] Add `searchField` to query the `searchLayer` field
  - [ ] Remove `mapServiceURL`
  - [ ] Remove `searchLayerIndex`
- [ ] Replace all refereces to `agrc/widgets/locate/FindGeneric` with `MagicZoom`
