# Camera calibration and Pose estimation에 기능 추가하기


챗지피티를 활용하여 chessboard를 calibration하여 다른 도형을 출력하는 과제를 배경으로
q, e 키보드 입력을 통해 3개의 박스를 두고 이전 박스, 이후 박스로 순환하는 방법을 추가해 보았다.

Camera calibration을 통해 알아낸 값은 


* Camera matrix (K) = [[1.97515271e+03, 0.00000000e+00, 5.49311781e+02],
 [0.00000000e+00, 1.97406482e+03, 9.45237271e+02],
 [0.00000000e+00, 0.00000000e+00, 1.00000000e+00]]
 
 
* Distortion coefficient (k1, k2, p1, p2, k3, ...) = [ 0.01044914, -1.07101685, -0.00143694, -0.01396964,  1.3123999 ]

Camera calibration 과 Pose estimation은 기본적으로 거의 비슷하고 체스판의 크기와 일부 코드를 수정 및 추가했다.

추가로 체스판의 화면이 너무 크게나오는 부분 때문에 화면 크기를 조정했다.

변경 및 추가된 코드는 다음과 같다.
```
# Define initial 3D box shapes
box_lower_1 = board_cellsize * np.array([[4, 2,  0], [7, 2,  0], [7, 4,  0], [6, 4,  0], [6, 3,  0], [4, 3,  0]])
box_upper_1 = board_cellsize * np.array([[4, 2, -1], [7, 2, -1], [7, 4, -1], [6, 4, -1], [6, 3, -1], [4, 3, -1]])

box_lower_2 = board_cellsize * np.array([[1, 2,  0], [2, 3,  0], [2, 5,  0], [1, 3,  0]])
box_upper_2 = board_cellsize * np.array([[1, 2, -1], [2, 3, -1], [2, 5, -1], [1, 3, -1]])

box_lower_3 = board_cellsize * np.array([[2, 5,  0], [3, 4,  0], [2, 4,  0], [2, 5,  0]])
box_upper_3 = board_cellsize * np.array([[2, 5, -1], [3, 4, -1], [2, 4, -1], [2, 5, -1]])

# Define previous 3D box shapes
prev_box_lower = np.copy(box_lower_1)
prev_box_upper = np.copy(box_upper_1)

# Create a named window
cv.namedWindow('Pose Estimation (Chessboard)', cv.WINDOW_NORMAL)
cv.resizeWindow('Pose Estimation (Chessboard)', 1080, 960)
```


```
 # Check if 'q' key is pressed to change the box shape
        key = cv.waitKey(1)
        if key == ord('q'):
            # Cycle through the box shapes
            prev_box_lower = np.copy(box_lower_1)
            prev_box_upper = np.copy(box_upper_1)
            if box_index == 1:
                box_lower = np.copy(box_lower_2)
                box_upper = np.copy(box_upper_2)
                box_index = 2
            elif box_index == 2:
                box_lower = np.copy(box_lower_3)
                box_upper = np.copy(box_upper_3)
                box_index = 3
            elif box_index == 3:
                box_lower = np.copy(box_lower_1)
                box_upper = np.copy(box_upper_1)
                box_index = 1
        elif key == ord('e'):
            # Cycle through the box shapes
            prev_box_lower = np.copy(box_lower_1)
            prev_box_upper = np.copy(box_upper_1)
            if box_index == 1:
                box_lower = np.copy(box_lower_3)
                box_upper = np.copy(box_upper_3)
                box_index = 3
            elif box_index == 2:
                box_lower = np.copy(box_lower_1)
                box_upper = np.copy(box_upper_1)
                box_index = 1
            elif box_index == 3:
                box_lower = np.copy(box_lower_2)
                box_upper = np.copy(box_upper_2)
                box_index = 2
                ```


###해당 영상은 q,e 키를 이용해 3개의 박스를 이전 박스 또는 이후 박스로 바꾸는 기능이 추가된 영상이다.

https://github.com/29pyg/ARbox/assets/61642764/19a84476-e3ec-41f3-8d36-a2e31f352934

