# Correcting-for-Distortion-Image

Image distortion occurs when a camera looks at 3D objects in the real world and transforms them into a 2D image; this transformation isn’t perfect. Distortion actually changes what the shape and size of these 3D objects appear to be. So, we first analyzing camera images, is to undo this distortion so that you can get correct and useful information out of them.

I Used Chessboard pattern, and taken 36 pictures of it.

Some are
![gopr0064](https://user-images.githubusercontent.com/22203782/50425708-3ea16700-08a2-11e9-97b6-1e5b88c2617a.jpg)
![gopr0053](https://user-images.githubusercontent.com/22203782/50425709-3ea16700-08a2-11e9-8bb2-3d0475c93276.jpg)
![gopr0055](https://user-images.githubusercontent.com/22203782/50425710-3f39fd80-08a2-11e9-8147-04eaec2e5df7.jpg)
![gopr0058](https://user-images.githubusercontent.com/22203782/50425711-3f39fd80-08a2-11e9-8a73-f8e2d5e3cdfd.jpg)


Count the number of corners in any given row. Similarly, count the number of corners in a given column. Keep in mind that "corners" are only points where two black and two white squares intersect, in other words, only count inside corners, not outside corners.

### Used the OpenCV functions findChessboardCorners() to find Corners that takes grayscale image ( Convert using cv2.cvtColor(img, cv2.COLOR_BGR2GRAY) )

## No of corners in rows -> 6 
## No of corners in coloum -> 8 

### Prepare object points and image points and store in a list
_objpoints = [] # 3D points_
_imgpoints = [] # 2D points_

_objp = np.zeros((6*8,3), np.float32)_
_objp[:,:2] =  np.mgrid[0:8,0:6].T.reshape(-1,2) # x, y coordinates_

_objpoints.append(objp)_
_imgpoints.append(corners) # findChessboardCorners() returns corners_
        
![download](https://user-images.githubusercontent.com/22203782/50425786-2d0c8f00-08a3-11e9-9247-2a8db90a4423.png)

### Used the OpenCV functions drawChessboardCorners() to draw corners in an image of a chessboard pattern.

To learn more about both of those functions, look at the OpenCV documentation here: [cv2.findChessboardCorners()] (https://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#cv2.drawChessboardCorners) and [cv2.drawChessboardCorners()](https://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#cv2.findChessboardCorners).

### To calibrate Camera used cv2.calibrateCamera()
It takes arguments objpoints and imgpoints and return distortion coefficients (dist), camera matrix (mtx) that we need to transform 3D object points to 2D image points. It also returns the position of the camera in the world, with values for rotation (rvecs) and translation (tvecs) vectors

_ret, mtx, dist, rvecs, tvecs = cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1], None, None)_

### To covert image used cv2.undistort()
_dst = cv2.undistort(img, mtx, dist, None, mtx)_

## Tested on test image
![screenshot 197](https://user-images.githubusercontent.com/22203782/50425900-df455600-08a5-11e9-880e-ce6b4d7d2b9b.png)
