# PNG Web Service 
Front-end web interface to hide and extract GIFs from PNG images

<img src=https://imgur.com/pdwe4Sq.png/ width="750px">

***
## Description
A PNG file is composed of a fixed 8-byte signature header, followed by the PNG file’s contents organized into chunks. Each chunk consists of 4 parts: `length`, `type`, `data`, `CRC` (Cyclic Redundancy Check). When reading a PNG file’s contents, if a chunk’s `type` is not recognized as one of the 25 pre-defined types outlined in the PNG specification ([See Specification](https://www.w3.org/TR/png/#abstract)), it is ignored as long as the chunk’s `CRC` is valid. 

Thus by inserting chunks with an unrecognized `type` and creating a valid `CRC` for our chunk, we are able to hide and extract the byte data of any file from a PNG image. And if a PNG file with a hidden package were to be opened, it would appear as any ordinary PNG image. In this project, the code written to insert and extract GIFs from PNG files. 

I have created a simple front-end web interface to interface with a PNG processing backend using HTTP Request-Response communication. 

***
## Usage 
Launch the service by starting the flask application:
- Windows: `py -m flask run`
- macOS: `python3 -m flask run` 
***
## How it works
To see the 'chunks' of data stored in the PNG file, run `./png-analyze <filename>` from the microservice 
### Example output of `./png-analyze sample.png`
```
PNG Header: OK
Chunk: IHDR (13 bytes of data)
Chunk: SRGB (1 bytes of data)
Chunk: eXIf (268 bytes of data)
Chunk: pHYs (9 bytes of data)
Chunk: iTXt (8962 bvtes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (4578 bytes of data)
Chunk: IEND (0 bytes of data)
```
To manually insert a file into a PNG, run `./png-hide <pngSrcFilename> <filenameToHide> <pngDstFilename>`
### Example output of `./png-analyze sample.png` with hidden file
``` 
PNG Header: OK
Chunk: IHDR (13 bytes of data)
Chunk: SRGB (1 bytes of data)
Chunk: eXIf (268 bytes of data)
Chunk: pHYs (9 bytes of data)
Chunk: file (20415 bytes of data)
Chunk: iTXt (8962 bvtes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (16384 bytes of data)
Chunk: IDAT (4578 bytes of data)
Chunk: IEND (0 bytes of data)
```
The byte data of the inserted file is stored in the 'file' chunk of the PNG. 
To extract the data back from the inserted file, run `./png-extract <pngFilename> <dstFilename>` 


