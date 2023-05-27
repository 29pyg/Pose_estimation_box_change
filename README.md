# Something tracking


챗지피티를 활용하여 영상에서 움직임을 가지는 물체를 표시하고 가능하면 추적하는 프로그램을 구현해보고자 한다.


이후 가능하면 영상을 만화처럼 영상체?라고 해야하나 바꾸는 기능을 추가하여 그때도 추적할 수 있도록 해보고자 한다.

첫 시작 질문이다.

![질문과 대답1](https://github.com/29pyg/ARbox/assets/61642764/5a030b2c-c1fc-4b08-b9c1-c5c9e2763af1)

## chat gpt가 준 코드는 다음과 같다.

import cv2

# 동영상 파일 열기
video = cv2.VideoCapture('동영상파일.mp4')

# 동영상 프레임의 첫 번째 프레임 읽기
ret, frame = video.read()

# 추적할 물체 선택
bbox = cv2.selectROI(frame, False)

# 추적기 초기화
tracker = cv2.TrackerCSRT_create()
tracker.init(frame, bbox)

while True:
    # 다음 프레임 읽기
    ret, frame = video.read()
    
    # 동영상이 끝나면 종료
    if not ret:
        break
    
    # 추적기 업데이트
    success, bbox = tracker.update(frame)
    
    # 추적 성공 시, 추적된 물체에 사각형 그리기
    if success:
        (x, y, w, h) = [int(v) for v in bbox]
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
    
    # 추적된 동영상 프레임 보여주기
    cv2.imshow('Object Tracking', frame)
    
    # 'q' 키를 누르면 종료
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 자원 해제
video.release()
cv2.destroyAllWindows()

