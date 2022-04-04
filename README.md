# 3DTerrian
**3차원 지형을 Light와 Camera, Material을 활용**

8팀 : 김기현, 서창환, 신영룡

---
### 변화 적용시킨 부분

1. 3D지형에 `box` 생성
2. `box`에 사진 넣기
3. 마우스로 3D 스케치 주위를 움직이기
4. `box`에 빛 반사 일으키기

---
### 3D 지형 생성

##### 2차원 배열 생성
```
var cols, rows;
var scl = 20;
var w = 1400; //가로 크기
var h = 1000; // 세로 크기
var terrain = []; // 1차원 배열
function setup() {
  createCanvas(600, 600, WEBGL);
  cols = w / scl; // 가로 7칸
  rows = h / scl; // 세로 5칸

  for (var x = 0; x < cols; x++) {
    terrain[x] = []; // 세로로 기둥 7개 
    /*for (var y = 0; y < rows; y++) {
     terrain[x][y] = 0; //specify a default value for now
     }*/ 
  } // 2차원 배열 생성
  noStroke(); // 선이나 윤곽선을 보이지 않게함
}
```
#### 3D 지형 만들기
```
flying -= 0.1; // Z축 값 변함
var yoff = flying;
for (var y = 0; y < rows; y++) {
  var xoff = 0;
  for (var x = 0; x < cols; x++) {
    terrain[x][y] = map(noise(xoff, yoff), 0, 1, -100, 100); // noise로 받아온 값의 범위가 0 ~ 1을 -100 ~ 100으로 mapping
    xoff += 0.2;
  }
  yoff += 0.2;
} // 점들이 위 아래로  랜덤으로 이동 -> 3차원 
  
background(10, 10, 250);
orbitControl();
translate(0, 50);
rotateX(PI / 3); // 대각선 기울림
fill(200, 200, 200, 150);
translate(-w /2, -h / 2 );
for (var y = 0; y < rows - 1; y++) {
  beginShape(TRIANGLE_STRIP); // 여러개 삼각형을 연결해 한줄로 그림
  for (var x = 0; x < cols; x++) {
    let v = terrain[x][y];
    v = map(v, -100, 100, 0, 255);
    fill(v-110, v-20, v-110); // 지형 색상 변경 
    vertex(x * scl, y * scl, terrain[x][y]); 
    vertex(x * scl, (y + 1) * scl, terrain[x][y + 1]);
  } // y축 현재점과 다음 밑에 y축 연결
    // z축 현재점과 다음 밑에 z축 
  endShape();
} 
```
---
### 3D 지형에 `box` 생성

`box(200);`

---

### `box`에 사진 넣기

* 이미지 파일 불러오기
```
function preload() {
  img = loadImage('Corgi.jpg');
}
```
* 이미지를 텍스처 랜더링
  * function draw()에
`texture(img);`

---

### 마우스로 3D 스케치 주위를 움직이기
* 카메라 시점 이동  
  * function draw()에
`orbitControl();` // 마우스 또는 트랙 패드로 3D 스케치 주위를 움직일 수 있습니다. 

---

### `box`에 빛 반사 일으키기
* `box`를 반사시키기 위한 `pointLight`

```
let locX = mouseX - width / 2;
let locY = mouseY - height / 2;
pointLight(255, 255, 255, locX, locY, 50); //색상과 조명 위치를 갖는 포인트 라이트
```
* 빛 반사
`specularMaterial(250);` // 빛 반사 일으키기
* 광택
`shininess(50);` // 셰이더 표면의 광택 양을 설정

---

### `box` 돌리기

`let theta = 0;` // 각도 0도

`rotateZ(theta * 0.1);` // z축을 따라 회전합니다.  
`rotateX(theta * 0.1);` // x축을 따라 회전합니다.  
`rotateY(theta * 0.1);` // y축을 따라 회전합니다.  
`theta += 0.05;`


---
### 구현
![3DTerrain 구현](corgimove.gif)
---
  
### 소감

김기현

3D 지형을 만들고 박스를 만들고 변형 시켜보면서 진정한 그래픽스 실습을 한 것 같습니다.  
팀원들과 같이 작업을 하다보니까 프로젝트가 수월하게 진행되어 좋았습니다. 앞으로도 수업 열심히 듣고  
javascript는 제 것으로 만들겠습니다!

서창환

과제를 수행하는데 많은 어려움이 있을뻔 했지만 팀원들의 도움을 받아 보다 수월하게 학습할수 있었습니다.
프로젝트를 수행하는 과정에서 레퍼런스를 찾아보며 자바스크립트를 더욱 공부하여 다양한 응용을 하고싶다는 생각이 들었습니다.
거기에 더해 팀과제를 통해 깃허브의 사용법까지 배울수있어 얻어갈 것이 많은 시간이였습니다. 감사합니다.

신영룡

처음에 혼자 개인 과제를 수행할 때는 3D 지형 만들기가 조금 어려워서 힘들었지만, 팀원들과 함께 
팀 프로젝트를 수행하다 보니 서로 어려운 부분을 알려주어서 좀 더 수월하게 배울 수 있었습니다.
프로세싱이 활용도가 이렇게 높은지 몰랐는데 놀랍고 재밌습니다.
이번 과목 목표는 이렇게 배운 것들을 활용해서 나만의 게임을 만드는 것이 목표입니다.
