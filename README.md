
<a href="https://mitwpu.edu.in"><img src="https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/mit.png" height=100px></a><a href="https://www.sih.gov.in/"><img src="https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/sih.png" height=100px></a>

# Project Aletheia

## NM391 The Ones n Zeros

##### *NM391-ISRO Reconstruction of missing data in Satellite Imagery* : Short Wave Infra-Red(SWIR) detectors used in satellite imaging cameras suffer from dropouts in pixel and line direction in raw data. Develop software to reconstruct missing parts of a satellite image so that observers are unable to identify regions that have undergone reconstruction. 

## Smart India Hackathon 2020

### Satellite Imagery

- Satellite imagery depicts the Earth’s surface at various spectral, temporal, radiometric, and increasingly detailed spatial resolutions, as is determined by each collection system’s sensing device, and the orbital path of its reconnaissance platform.

- Satellite images are one of the most powerful and important tools used by the meteorologist. They are essentially the eyes in the sky. These images reassure forecasters to the behavior of the atmosphere as they give a clear, concise, and accurate representation of how events are unfolding.

- There are three main types of Satellite Imagery used today : 

	* *VISIBLE IMAGERY* : Visible images represent the amount of sunlight being scattered back into space by the clouds, aerosols, atmospheric gases, and the Earth's surface.

	* ***INFRARED IMAGERY¹ : IR or infrared satellite imagery is sort of a temperature map. The weather satellite detects heat energy in the infrared spectrum (infrared energy is invisible to the human eye). Since temperature, in general, decreases with increasing height, high altitudes will appear whiter than low altitudes.***
	
	* *WATER VAPOUR IMAGERY* : Water vapor satellite pictures indicate how much moisture is present in the upper atmosphere (approximately from 15,000 ft to 30,000 ft). The highest humidities will be the whitest areas while dry regions will be dark. Water vapor imagery is useful for indicating where heavy rain is possible.
	
¹ For the sake of efficiency, we shall constrain our analysis to INFRARED IMAGERY only, in particular, Short Wave Infrared Imagery or SWIR.

### Short Wave Infrared Images (SWIR)

- SWIR refers to non-visible light falling roughly between 1400 and 3000 nanometers (nm) in wavelength. Visible light, on the other hand, typically corresponds to the 400 to 700 nm range. Immediately adjacent to visible light is near infrared, or NIR, within the 700 to 1400 nm range, and SWIR is adjacent to NIR.

- Collecting satellite imagery in SWIR wavelengths has unique benefits, including improved atmospheric transparency and material identification. Because of their chemistries, many materials have specific reflectance and absorption features in the SWIR bands that allow for their characterization from space.

### Errors in SWIR

- A digital remotely sensed image is typically composed of picture elements (pixels) having Digital Number (DN) or Brightness Value (BV) located at the intersection 
of each row and column of each band in imagery. A smaller number indicates low radiance from the area and high number is an indicator of high radiant properties of the area. Raw digital images usually contain distortions or various types of error so they cannot be used directly without proper processing. Sources of these distortions range from variation in the altitude and velocity of the sensor to Earth's rotation and curvature etc. Corrections are thus necessary for such satellite images. Programs to rectify Satellite imagery aim to correct distorted or degraded image data to create a reliable representation of the original scene.

- Broadly Categorised, there are two types of errors, Radiometric and Geometric Errors, for the scope of the project we shall be limited to Radiometric Errors involving missing data.

- Radiometric Errors are caused by inconsistencies in the capturing sensor and atmospheric conditions which lead to incorrect or no brightness values being recorded.

- Such errors include : 

	* Random bad pixels (shot noise). 
	
		<img src="https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/badpixerr.jpg" height=300px>
	
	* Line-start/stop problems. 
	
		<img src="https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/linestarterr.jpg" height=300px>
	
	* ***Line or column drop-outs.*** ²
	
		<img src="https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/coldropouterr.jpg" height=300px>
	
	* Partial line or column drop-outs.
	
		<img src="https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/partialdropouterr.jpg" height=300px>
	
	* Line or column striping.
	
		<img src="https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/stripingerr.jpg" height=300px>


² In order to have a fixed scope to facilitate easier understanding, we shall be constrained to Line & Column Dropouts only for Data Reconstruction.


### Reconstruction

- The process of building or forming (something) again after it has been damaged or destroyed is known as reconstruction, in this case we shall refer to reconstructing the missing data in Satellite Images caused by Radiometric Errors.


## Proposed Solution

#### Traditional methods
Traditional methods of rectifying the errors in question are listed as follows : 

- Replacement by others Pixels in Vicinity :  the brightness value of the missing pixels is replaced by the value of the pixel on immediately preceding or succeeding line.
- Replacement by Averaging : the brightness value of the missing pixels is calculated by taking the mean of the pixels in its proximity.
- In essence, simple mathematical operations are used in order to estimate the missing data in terms of numerical values.

#### New Technique
The proposed technique we plan to implement aims to utilise the newly unveiled power of Machine Learning and Artificial Intelligence to provide Optimum, Efficient and Accurate rectification methods by reconstructing the missing data using Neural Networks.

# Realisation & Implementation of Proposed Solution

## Parsing Satellite Imagery

- The first challenge faced by our team was reading the satellite Raster Data and converting it into a Programmatically Modifiable format in order for our Neural Network to be able to Reconstruct the Missing Data.

- In order to accommodate this our first solution was to read the binary data from the raw Raster via native python methods and to convert it into a Numpy Matrix with pixel values indicating the Luminosity of each pixel. Which gave rise to the problem of having unsigned 16 bit integers as the datatype for the image.

- Having 16-bit Images made it difficult for openly Available solutions to be able to read the data, hence we came up.with a conversion process to convert the data to unsigned 8 bit integers.

```
>>> matrix16
array([[1265, 1245, 1274, ..., 1286, 1291, 1220],
       [1297, 1282, 1272, ..., 1265, 1293, 1240],
       [1306, 1302, 1327, ..., 1254, 1311, 1269],
       ...,
       [1181, 1146, 1169, ..., 1274, 1277, 1240],
       [1251, 1189, 1203, ..., 1314, 1271, 1228],
       [1303, 1205, 1198, ..., 1223, 1209, 1171]], dtype=uint16)
>>> matrix8 = matrix16//256
>>> matrix8 = matrix8.astype(np.uint8)
>>> matrix8
array([[4, 4, 4, ..., 5, 5, 4],
       [5, 5, 4, ..., 4, 5, 4],
       [5, 5, 5, ..., 4, 5, 4],
       ...,
       [4, 4, 4, ..., 4, 4, 4],
       [4, 4, 4, ..., 5, 4, 4],
       [5, 4, 4, ..., 4, 4, 4]], dtype=uint8)
```

- This caused a reduction in data thus making our solution less reliable and preferable at its job. Creating a new challenge for us.

- ***In order to have a reliable and efficient solution, we reverted back to binary parsing and kept the data to its Predefined 16 bit format and Started working on a neural network which would be able to process this 16 bit data.***

- ***This eliminated the option for us to borrow and reference code from other commercial and free solutions being used to reconstruct day to day images as they supported only 8 bit data.***

## Developing a Neural Network for the Reconstruction

### Citations
```
@article{yu2018free,
  title={Free-Form Image Inpainting with Gated Convolution},
  author={Yu, Jiahui and Lin, Zhe and Yang, Jimei and Shen, Xiaohui and Lu, Xin and Huang, Thomas S},
  journal={arXiv preprint arXiv:1806.03589},
  year={2018}
}
```


### Instructions for dropouts_mask.py : 

- Syntax : `python dropouts_mask.py (input_file) [OPTIONAL = output_extension png|tiff|tif]`


[mitlogo]: https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/mit.png
[sihlogo]: https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/sih.png

[badpixerr]: https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/badpixerr.jpg
[coldropouterr]: https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/coldropouterr.jpg
[linestarterr]: https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/linestarterr.jpg
[partialdropouterr]: https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/partialdropouterr.jpg
[stripingerr]: https://github.com/its-charan-here/NM391_The-Ones-n-Zeros/blob/master/.README_files/stripingerr.jpg