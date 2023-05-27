# Something tracking


챗지피티를 활용하여 영상에서 움직임을 가지는 물체를 표시하고 가능하면 추적하는 프로그램을 구현해보고자 한다.


이후 가능하면 영상을 만화처럼 영상체?라고 해야하나 바꾸는 기능을 추가하여 그때도 추적할 수 있도록 해보고자 한다.

첫 시작 질문이다.

![image](https://github.com/29pyg/ARbox/assets/61642764/b63494eb-f510-4ac2-8798-8e6a5debfce6)


chat gpt의 코드는 다음과 같다.
 ```
 import cv2

# 배경 차분(Background Subtraction) 모델 생성
background_subtractor = cv2.createBackgroundSubtractorMOG2()

# 동영상 파일 열기
video = cv2.VideoCapture('동영상파일.mp4')

# 추적할 물체 초기화
tracker = None
bbox = None

while True:
    # 다음 프레임 읽기
    ret, frame = video.read()
    
    # 동영상이 끝나면 종료
    if not ret:
        break
    
    # 배경 차분 수행하여 이진 이미지 생성
    mask = background_subtractor.apply(frame)
    mask = cv2.threshold(mask, 200, 255, cv2.THRESH_BINARY)[1]
    
    if tracker is None:
        # 초기 추적기 설정
        contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
        
        for contour in contours:
            area = cv2.contourArea(contour)
            if area > 500:  # 임계값을 조정하여 작은 노이즈 제거
                x, y, w, h = cv2.boundingRect(contour)
                bbox = (x, y, w, h)
                tracker = cv2.TrackerCSRT_create()
                tracker.init(frame, bbox)
                break
    else:
        # 추적기 업데이트
        success, bbox = tracker.update(frame)
        
        if success:
            # 추적 성공 시, 추적된 물체에 사각형 그리기
            (x, y, w, h) = [int(v) for v in bbox]
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
        
    # 동영상 프레임 보여주기
    cv2.imshow('Object Tracking', frame)
    
    # 'q' 키를 누르면 종료
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 자원 해제
video.release()
cv2.destroyAllWindows()

  ```
