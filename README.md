Caltech Pedestrian Dataset Converter
============================

# Requirements

- OpenCV 3.0+ (with Python binding)
- Python 2.7+, 3.4+, 3.5+
- NumPy 1.10+
- SciPy 0.16+

# Caltech Pedestrian Dataset

```
$ bash shells/download.sh
$ python scripts/convert_annotations.py
$ python scripts/convert_seqs.py
```

Each `.seq` movie is separated into `.png` images. Each image's filename is consisted of `{set**}_{V***}_{frame_num}.png`. According to [the official site](http://www.vision.caltech.edu/Image_Datasets/CaltechPedestrians/), `set06`~`set10` are for test dataset, while the rest are for training dataset.

(Number of objects: 346621)

# Draw Bounding Boxes

```
$ python tests/test_plot_annotations.py
```

# Structure of produced annotations.json
- set_name: string
  - video_name: string
    - nFrame: integer
    - maxObj: integer
    - log: list
    - logLen: integer
    - altered: integer
    - frames: dict<frame_id (string), list(dict)>
        - e.g. "186": [
```
		{"id": 24,  
		"pos": [0.5, 203.49978594529648, 17.252551400712175, 54.99802446911718],  
		"occl": 1,  
		"lock": 0,  
		"posv": [3.5590197431629655, 203.49978594529648, 14.173593948019912, 34.83144384949176],  
		"lbl": "person",  
		"str": 114,  
		"end": 244,  
		"hide": 0,  
		"init": 1},  

		{"id": 3,  
		"pos": [591.6391446433643, 177.68202614379086, 34.940537084399125, 85.80129298096054],  
		"occl": 0,  
		"lock": 0,  
		"posv": [0, 0, 0, 0],  
		"lbl": "person",  
		"str": 1,  
		"end": 759,  
		"hide": 0,  
		"init": 1},  
		]  
```

# Notes:
  - `pos`, `posv` and `occl`  
	  - The visible region of an object (posv) should be a subset of the predicted region (pos)
	  - If the object is not occluded, then posv is the same as pos (posv=[0 0 0 0] by default and indicates posv not set).
	  - If occl==0 for a given bb, posv is set to [0 0 0 0]. If occl==1 and posv=[0 0 0 0], posv is set to pos.
  - `lbl`: a string label describing object type (eg: 'pedestrian')
  - `str`  - the first frame in which object appears (1 indexed)
  - `end`  - the last frame in which object appears (1 indexed)
  - `hide` - 0/1 value indicating object is 'hidden' (used during labeling)
  - `init` - 0/1 value indicating whether object w given id exists  
  - `lock` - 0/1 value indicating bb is 'locked' (used during labeling)  


