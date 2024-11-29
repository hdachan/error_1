# Flutter Frontend 오류 모음

이 문서는 Flutter에서 발생한 여러 가지 오류와 그 해결 방법을 정리한 것입니다.

## 1. 발생한 오류
LayoutBuilder(
  builder: (context, constraints) {
    final String longText = '개그우먼 홍현희의 아들 준범이는 평소에 어떻게 입을까요? 저희 모디랑이 찾아 봤습니다. 관련 매거진랑 똑같이 '
        '개그우먼 홍현희의 아들 준범이는 평소에 어떻게 입을까요? 저희 모디랑이 찾아 봤습니다. 관련 매거진랑 똑같이';

    final textPainter = TextPainter(
      text: TextSpan(
        text: longText,
        style: TextStyle(
          color: Color(0xFF3D3D3D),
          fontSize: 14,
          fontFamily: 'Pretendard',
          fontWeight: FontWeight.w400,
          height: 1.3,
          letterSpacing: -0.35,
        ),
      ),
      maxLines: 2,
      textDirection: TextDirection.ltr, // 이 줄을 제거하여 오류를 피할 수 있습니다.
    );

    textPainter.layout(maxWidth: constraints.maxWidth);
    bool isTextOverflowing = textPainter.didExceedMaxLines;

    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          longText,
          style: TextStyle(
            color: Color(0xFF3D3D3D),
            fontSize: 14,
            fontFamily: 'Pretendard',
            fontWeight: FontWeight.w400,
            height: 1.3,
            letterSpacing: -0.35,
          ),
          maxLines: _isExpanded ? null : 2,
          overflow: _isExpanded ? null : TextOverflow.ellipsis,
        ),
        SizedBox(height: 8),
        if (!_isExpanded && isTextOverflowing) ...[
          SizedBox(height: 4),
          GestureDetector(
            onTap: () {
              setState(() {
                _isExpanded = !_isExpanded;
              });
            },
            child: Container(
              width: 36,
              child: Align(
                alignment: Alignment.bottomCenter,
                child: Text(
                  _isExpanded ? '접기' : '더보기',
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    color: Color(0xFFB0B0B0),
                    fontSize: 14,
                    fontFamily: 'Pretendard',
                    fontWeight: FontWeight.w400,
                    height: 1.3,
                    letterSpacing: -0.35,
                  ),
                ),
              ),
            ),
          ),
        ],
      ],
    );
  },
),

### 오류 메시지
이 오류는 `TextDirection.ltr`을 사용할 때 발생했습니다. 이는 `TextDirection`이 제대로 정의되지 않았거나, 올바르게 import되지 않았음을 의미합니다.

## 2. 해결 방법

------------------------------------------------
### 1번 방법: UI 라이브러리
import 'dart:ui' as ui; 추가

### 2번 방법: intl 패키지 수정
가끔 `intl` 패키지와의 충돌로 인해 `TextDirection` 관련 오류가 발생할 수 있습니다. 이 경우, 다음과 같이 import 문을 수정하여 문제를 해결할 수 있습니다.
기존 import 'package:intl/intl.dart';
수정된 라이브러리 import 'package:intl/intl.dart' as intl;

### 3번 방법: 클래스 충돌 해결
클래스 간의 충돌이 발생하는 경우, 해당 클래스를 재구성하거나 이름을 변경하여 문제를 해결합니다. 예를 들어, 하단바를 클래스로 불러오는 경우, 다른 이름을 사용하여 충돌을 피할 수 있습니다.

