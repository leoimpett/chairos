# CHAIROS 

## Image Search Protocol



Many computer vision algorithms provide some kind of measure of image similarity. This enables users to find similar images (i.e. search by images), but also to search for similar details of a particular image. 



Some example systems:

[Visual Image Search Engine](http://www.robots.ox.ac.uk/~vgg/software/vise/), Visual Geometry Group, University of Oxford

[Visual Search Tool](https://hciweb.iwr.uni-heidelberg.de/compvis/projects/visualSearch), Heidelberg Image Collaboratory, University of Heidelberg

[REPLICA Search Engine](https://diamond.timemachine.eu/), Digital Humanities Lab, EPFL



Broadly, they follow the same workflow:

1. System ingests and (pre-)process all images in a dataset

2. User selects an image, or a rectangular crop from an image

3. System compares the query image to the pre-ingested images

4. System displays a grid of similar images

5. (Optional) User refines the search with further positive or negative queries, and recieves different results

   



![image-20191002190352804](wireflow-imsearch.png)





The IIIF Protocol allows us to: 

1. Efficiently ingest datasets of image into the dataset from different collections
2. Use a IIIF viewer to select a detail to search (e.g. using something similar to the Rectangular Annotation tool in Mirador)
3. Efficiently display thumbnails of the results and load high-resolution versions
4. Keep institutional metadata at every stage



Here are some examples for how we might deal with such a workflow:



### Ingesting Data 

```json
{'load': [{'manifest':'https://digi.vatlib.it/iiif/MSS_Vat.lat.3225/manifest.json', 
        'canvases':'all'}, 
       {'manifest':'https://iiif.lib.harvard.edu/manifests/drs:5981093', 
       'canvases':'all'}]
}
```



### Querying with an Image Region

```json
{'query':{
  'manifest':'https://iiif.harvardartmuseums.org/manifests/object/299843',
  'imurl':'https://ids.lib.harvard.edu/ids/iiif/47174896/full/full/0/native.jpg',
  'canvas':'https://iiif.harvardartmuseums.org/manifests/object/299843/canvas/canvas-47174896',
  'rectangle': [0.34, 0.675, 0.134, 0.9531]
	}
}
```



### Results

```json
{'results':[
{'manifest':'https://iiif.lib.harvard.edu/manifests/drs:5981093', 'canvas':'https://iiif.lib.harvard.edu/manifests/drs:5981093/range/range-0-6-1-3.json',
'rectangle': [0.1313, 0.7462, 0.1354, 0.7532],
'similarity': 0.642, 
'id':0},
{'manifest':'https://iiif.lib.harvard.edu/manifests/drs:5981093', 'canvas':'https://iiif.lib.harvard.edu/manifests/drs:5981093/range/range-0-6-1-7.json',,
'rectangle': [0.1354, 0.6423, 0.3541, 0.6436],
'similarity': 0.473, 
'id':1},
{'manifest':'https://digi.vatlib.it/iiif/MSS_Vat.lat.3225/manifest.json', 'canvas':'https://digi.vatlib.it/iiif/MSS_Vat.lat.3225/canvas/p0002',,
'rectangle': [0.2456, 0.7420, 0.3567, 0.9302],
'similarity': 0.136, 
'id':2}],
'query_id':'wot3jt2b3keo'
}
```



### Refine Search

```json
{'refine':{
  'query_id':'wot3jt2b3keo',
	'positives':[0,1,5,7],
	'negatives':[2,6]}
}
```