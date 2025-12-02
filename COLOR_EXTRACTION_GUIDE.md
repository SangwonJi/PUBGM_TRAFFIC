# Finviz 색상 추출 가이드

## 방법 1: 브라우저 개발자 도구 사용 (가장 정확)

1. https://finviz.com/map.ashx 페이지를 엽니다
2. F12 또는 우클릭 > 검사로 개발자 도구를 엽니다
3. Elements 탭에서 히트맵 타일을 선택합니다
4. Styles 패널에서 `background-color` 또는 `color` 값을 확인합니다
5. 또는 Computed 탭에서 실제 렌더링된 색상을 확인합니다

## 방법 2: 이미지에서 색상 추출

1. Finviz 히트맵을 스크린샷으로 저장합니다
2. 이미지 편집 도구(Photoshop, GIMP 등)에서 색상 선택 도구로 픽셀 색상을 확인합니다
3. 또는 온라인 도구(예: https://imagecolorpicker.com/)를 사용합니다

## 방법 3: JavaScript로 색상 추출

브라우저 콘솔에서 실행:

```javascript
// 히트맵 타일 요소 선택
const tiles = document.querySelectorAll('[style*="background"]');
tiles.forEach(tile => {
  const bgColor = window.getComputedStyle(tile).backgroundColor;
  console.log(bgColor);
});
```

## 현재 적용된 Finviz 색상 팔레트

현재 코드에 이미 Finviz와 동일한 색상 팔레트가 적용되어 있습니다:

### 상승 그라데이션
- 약한 상승 (+0~1%): `#39D353`
- 보통 상승 (+1~2%): `#00C853`
- 큰 상승 (+2~3%): `#00E676`
- 급등 (≥+3%): `#00FF00`

### 하락 그라데이션
- 약한 하락 (-0~1%): `#FF6E6E`
- 보통 하락 (-1~2%): `#FF3B3B`
- 큰 하락 (-2~3%): `#FF1A1A`
- 급락 (≤-3%): `#FF0000`

### 중립
- 중립 (-0.5% ~ +0.5%): `#3D444C`

### 배경 및 경계선
- 배경: `#2B2F3A`
- 타일 경계선: `#1E232C`
- 섹터 경계선: `#FFFFFF` (패딩으로 표현)

