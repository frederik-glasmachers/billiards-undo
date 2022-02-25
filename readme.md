# Billiards-Undo

Simple html app wich displays overlays or differences of a video stream and a reference snapshot taken with a camera.
With an usb camera placed over the billiard table and a laptop next to it, this app aids with the placement of the balls to a previous position, in case you want to redo the previous shot.

Follow the online [link](https://frederik-glasmachers.github.io/billiards-undo/billiards-undo.html?offset=0.5&curFactor=0.5&refFactor=-0.5) or clone or download the repository and open the file billiards-undo.html with your favourite browser. Now move something in front of the camera. If you do not see anything try to adjust the camera resolution [settings](#settings).

## Taking a reference snapshot

When the app is started the snapshot reference frame is updated (a snapshot is taken) automatically.
Additional updates can be triggered by pressing any key or the left mouse button.
For this reason it is recommended to put a wireless mouse in your pocket.

## Settings

Settings are passed as [query strings](https://en.wikipedia.org/wiki/Query_string) like this:

    https://frederik-glasmachers.github.io/billiards-undo/billiards-undo.html?width=1280&height=720&brightness=2&offset=0.5&curFactor=0.5&refFactor=-0.5

| Id           | Default | Description                                                 |
| ------------ | ------- | ----------------------------------------------------------- |
| `width`      | 1280    | Width of the camera video stream requested                  |
| `height`     | 720     | Height of the camera video stream requested                 |
| `brightness` | 2       | Brightness of the camera video stream requested             |
| `offset`     | 0.5     | Offset to each color                                        |
| `curFactor`  | 0.5     | Factor applied to the current video stream frame            |
| `refFactor`  | -0.5    | Factor applied to the reference snapshot frame              |

For cameras which do not support the resolution of 1280x720 it is recommended to change the parameters `width` and `height`.

Color values are in the range of 0-1. Computation of a pixel is done in each color channel with the following formula:

    offset + curFactor*curImgColor + refFactor*refImgColor

Where `curImgColor` and `refImgColor` are the color values of the corresponding pixels from the current frame and the reference snapshot frame of the video stream.

So it is straight forward to adjust the [differential brightness](https://frederik-glasmachers.github.io/billiards-undo/billiards-undo.html?curFactor=1&refFactor=-1) or perform an [overlay](https://frederik-glasmachers.github.io/billiards-undo/billiards-undo.html?offset=0&curFactor=0.5&refFactor=0.5) operation insread of a difference, eventually with [trimmed brightness](https://frederik-glasmachers.github.io/billiards-undo/billiards-undo.html?offset=0&curFactor=1&refFactor=1) too.

## Implementation

Webgl is used to display the transformed video.
In order to simplify the code the library [twgl](https://twgljs.org/) in version 4 is used.
