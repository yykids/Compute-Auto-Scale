## Compute > Auto Scale > 콘솔 사용 가이드

## 동영상 가이드

<iframe width="560" height="315" src="https://www.youtube.com/embed/6ON5y6Dlk7w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 인스턴스 템플릿
### 인스턴스 템플릿 생성

오토스케일(auto scale)로 인스턴스를 생성하려면 먼저 인스턴스 템플릿을 작성해야 합니다.
인스턴스 템플릿을 작성할 때 필요한 항목은 다음과 같습니다.

<table class="it">
  <tr>
    <th>분류</th>
    <th>항목</th>
    <th>설명</th>
  </tr>
  <tr>
    <td rowspan="2">템플릿 정보</td>
    <td>이름</td>
    <td>인스턴스 템플릿의 이름</td>
  </tr>
  <tr>
    <td>설명</td>
    <td>인스턴스 템플릿의 설명. 영자 기준으로 최대 255자.</td>
  </tr>
  <tr>
    <td rowspan="8">인스턴스 정보</td>
    <td>이미지</td>
    <td>인스턴스 템플릿으로 생성될 인스턴스의 이미지.</td>
  </tr>
  <tr>
    <td>이름</td>
    <td>생성될 인스턴스의 이름.<br>같은 인스턴스 템플릿으로 생성된 인스턴스들은 모두 같은 이름을 가짐.</td>
  </tr>
  <tr>
    <td>가용성 영역(availability zone)</td>
    <td>인스턴스가 생성될 영역</td>
  </tr>
  <tr>
    <td>사양(flavor)</td>
    <td>생성될 인스턴스의 사양</td>
  </tr>
  <tr>
    <td>기본 디스크 크기</td>
    <td>생성될 인스턴스의 기본 디스크의 크기. <br>단위는 GB. 인스턴스의 사양에 따라 크기가 제한됨</td>
  </tr>
  <tr>
    <td>키페어(key pair)</td>
    <td>생성될 인스턴스에 접근하기 위한 키</td>
  </tr>
  <tr>
    <td>보안 그룹(security group)</td>
    <td>생성될 인스턴스의 보안 규칙</td>
  </tr>
  <tr>
    <td>네트워크</td>
    <td>생성될 인스턴스가 연결될 네트워크. <br>최대 4개까지 연결 가능하며, 연결 순서가 중요. <br>첫 번째 네트워크가 기본 게이트웨이 주소로 설정됨</td>
  </tr>
  <tr>
    <td rowspan="5">추가 정보</td>
    <td>플로팅 IP</td>
    <td>생성될 인스턴스에 플로팅 IP 할당 여부</td>
  </tr>
  <tr>
    <td>추가 디스크 이름</td>
    <td>생성될 인스턴스에 추가적으로 할당할 디스크의 이름</td>
  </tr>
  <tr>
    <td>추가 디스크 크기</td>
    <td>생성될 인스턴스에 추가적으로 할당할 볼륨의 크기<br> 단위는 GB이며, 10 ~ 1000GB 내의 값만 허용</td>
  </tr>
  <tr>
    <td>사용자 스크립트</td>
    <td>생성될 인스턴스에서 부팅 직후 실행할 스크립트.<br> 최대 영문 기준 65535자까지 허용</td>
  </tr>
  <tr>
    <td>Deploy 사용</td>
    <td>Deploy 서비스 연계 기능 사용 여부</td>
  </tr>
</table>

> [참고]
> 추가 디스크는 사용자 스크립트를 통해 마운트 과정을 거쳐야 사용할 수 있습니다. 사용자 스크립트를 통한 마운트 과정은 [블록 스토리지 가이드](/Storage/Block%20Storage/ko/overview/#_2)를 참고하시기 바랍니다.

<br/>

> [참고]
> Deploy 사용을 선택한 인스턴스 템플릿으로 스케일링 그룹을 생성하면 증설 시 자동으로 애플리케이션을 배포하도록 Deploy 서비스에 등록할 수 있습니다. 자세한 내용은 [Deploy 가이드](/Dev%20Tool/Deploy/ko/console-guide/)를 참고하세요.
>
> Deploy 사용 기능은 2020년 11월 현재 한국(판교), 한국(평촌), 일본(도쿄) 리전에서만 제공됩니다.

<br/>

> [주의]
> 인스턴스 템플릿은 한번 생성하면 수정할 수 없습니다.

## 스케일링 그룹
### 스케일링 그룹 목록 보기
현재 활성화된 스케일링 그룹을 보여줍니다. 목록 보기 화면에서는 각 스케일링 그룹의 상태를 확인할 수 있습니다.

- 최소/최대 인스턴스: 스케일링 그룹이 생성할 수 있는 최소/최대 인스턴스의 수입니다.
- 현재 인스턴스: 스케일링 그룹이 현재 보유 중인 인스턴스의 수입니다.
- 인스턴스 템플릿: 스케일링 그룹이 사용 중인 인스턴스 템플릿입니다.
- 로드 밸런서: 스케일링 그룹이 사용 중인 로드 밸런서입니다.
- 상태: 스케일링 그룹의 상태입니다. 스케일링 그룹의 정책 발동에 따른 동작 성공 여부를 확인할 수 있습니다. 스케일링 그룹의 상태 목록은 다음과 같습니다.

| 상태 | 설명 |
|--|--|
| CREATE_IN_PROGRESS | 스케일링 그룹 생성이 진행 중인 상태 |
| CREATE_COMPLETE | 스케일링 그룹 생성이 성공한 상태<br>구동 인스턴스 수만큼 인스턴스를 생성 |
| CREATE_FAILED | 스케일링 그룹 생성에 실패한 상태<br>고객 센터로 문의 |
| UPDATE_IN_PROGRESS | 스케일링 그룹에 대한 변화가 진행 중인 상태 |
| UPDATE_COMPLETE | 스케일링 그룹의 수정이나 확장/감축 정책이 발동되어 스케일링 그룹이 소유한 리소스에 변화가 생긴 상태 |
| UPDATE_FAILED | 스케일링 그룹에 대한 동작이 실패한 상태<br>고객 센터로 문의 |

### 스케일링 그룹 생성
스케일링 그룹에서는 다음과 같은 항목을 정의할 수 있습니다.

<table class="sg">
  <tr>
    <th>분류</th>
    <th>항목</th>
    <th>설명</th>
  </tr>
  <tr>
    <td rowspan="5">설정</td>
    <td>이름</td>
    <td>스케일링 그룹의 이름, 영어 대소문자, '-', '.' 및 숫자 20자 이내</td>
  </tr>
  <tr>
    <td>인스턴스 템플릿</td>
    <td>스케일링 그룹이 사용할 인스턴스 템플릿</td>
  </tr>
  <tr>
    <td>최소 인스턴스</td>
    <td>스케일링 그룹이 활성화되는 동안 최소한으로 유지될 인스턴스의 수</td>
  </tr>
  <tr>
    <td>최대 인스턴스</td>
    <td>스케일링 그룹이 활성화되는 동안 생성할 수 있는 최대 인스턴스의 수</td>
  </tr>
  <tr>
    <td>구동 인스턴스</td>
    <td>스케일링 그룹이 최초로 활성화되었을 때 생성되는 인스턴스의 수</td>
  </tr>
  <tr>
    <td rowspan="4">정책</td>
    <td>조건</td>
    <td>확장/감축 정책의 발동 조건<br>감시할 성능 지표, 기준값, 유지 시간을 지정</td>
  </tr>
  <tr>
    <td>조건 연산자</td>
    <td>발동 조건들 사이에 적용할 연산자<br>`and`를 선택하면 각 조건들을 모두 만족했을 때 정책 발동<br>`or`를 선택하면 조건들 중 하나만 만족해도 정책 발동</td>
  </tr>
  <tr>
    <td>인스턴스</td>
    <td>정책이 발동되었을 때 생성 혹은 삭제될 인스턴스의 수</td>
  </tr>
  <tr>
    <td>재사용 대기시간(cooldown time)</td>
    <td>정책이 발동된 후 다시 발동되기까지 기다려야 하는 시간<br>재사용 대기시간이 지나지 않았다면 조건을 만족해도 정책이 발동되지 않음</td>
  </tr>
  <tr>
    <td>로드 밸런서</td>
    <td>선택된 로드 밸런서</td>
    <td>생성된 인스턴스가 연결될 로드 밸런서</td>
  </tr>
</table>

### 로드 밸런서 변경
스케일링 그룹에 로드 밸런서를 연결하거나, 연결된 로드 밸런서를 제거, 교체할 수 있습니다. 연결할 로드 밸런서는 미리 생성되어 있어야 합니다.

> [참고]
> 이미 연결된 로드 밸런서에 리스너를 추가해도, 현재 스케일링 그룹의 인스턴스들이 자동으로 새 리스너에 연결되지는 않습니다.
> 새 리스너에 연결해야 한다면 로드 밸런서 연결을 해제했다가 다시 연결해야 합니다.

<br/>

> [주의]
> 로드 밸런서를 변경하면 현재 인스턴스는 삭제되고 새로운 인스턴스가 생성됩니다.

### 상세 정보 보기 및 수정
스케일링 그룹 목록에서 원하는 스케일링 그룹을 선택하여 상세 정보를 확인합니다.

상세 정보 화면에서 `편집`을 선택하면 스케일링 그룹의 속성을 수정할 수 있습니다. 스케일링 그룹을 수정하여 사용 중인 인스턴스 템플릿을 변경하거나 최소, 최대, 구동 인스턴스를 변경할 수 있습니다.

### 정책 보기 및 실행
스케일링 그룹 목록에서 원하는 스케일링 그룹을 선택해 스케일링 정책을 확인합니다.

스케일링 정책 화면에서 `편집`을 선택하면 스케일링 정책을 수정할 수 있습니다. 또한 확장/감축 정책에서 `실행`을 선택하여 강제적으로 정책을 발동할 수 있습니다.

### 예약 작업 보기 및 생성
스케일링 그룹 목록에서 원하는 스케일링 그룹을 선택하여 예약 작업을 확인합니다.

예약 작업을 통해 지정된 시간에 스케일링 그룹의 최소, 최대, 구동 인스턴스의 수를 조정할 수 있습니다.
예약 작업은 한 번만 실행하거나 주기적으로 실행하도록 설정할 수 있습니다.

예약 작업을 생성할 때 필요한 항목은 아래와 같습니다.

| 항목 | 설명 |
|--|--|
| 이름 | 예약 작업의 이름 |
| 반복 | 예약 작업의 반복 여부<br>1회 또는 Cron 표현식 중 하나를 선택 |
| Cron 표현식 | `반복`을 Cron 표현식으로 선택했을 때 활성화됨 |
| 기준 시간 | 예약 작업의 시작 시간 및 종료 시간에 대한 타임존 지정 |
| 시작 시간 | 예약 작업이 활성화될 시간<br>`반복`을 1회로 선택한 경우 시작 시간에 예약 작업 실행<br>`반복`을 Cron 표현식으로 선택한 경우 시작 시간을 기점으로 하여 주기적으로 예약 작업 실행 |
| 종료 시간 | 예약 작업이 종료될 시간<br>`반복`을 Cron 표현식으로 선택했을 때 활성화됨 |
| 변경 항목 | 예약 작업이 변경할 스케일링 그룹의 속성<br>최소, 최대, 구동 인스턴스 중 하나를 선택 |
| 변경 값 | 변경 항목에서 지정한 속성의 새로운 값<br>지정한 시간대에 `변경 항목`에서 선택한 속성을 이 값으로 수정 |

> [참고]
> Cron 표현식은 예약 작업의 실행 시간, 주기를 나타내기 위한 표현식입니다.
>
> Cron 표현식은 5개의 항목으로 구성되고 각 항목은 공백 문자로 구분됩니다. 항목별 의미는 다음과 같습니다.
>
> | 항목 | 허용 범위 | 사용 가능한 특수 문자 |
> |--|--|--|
> | 분 | 0-59 | `*` `,` `-` |
> | 시간 | 0-23 | `*` `,` `-` |
> | 일 | 1-31 | `*` `,` `-` `?` `L` `W` |
> | 월 | 1-12<br>JAN-DEC | `*` `,` `-` |
> | 요일 | 0-6<br>SUN-SAT | `*` `,` `-` `?` `L` `#` |
>
> 각 항목에는 숫자 또는 특수문자를 써서 실행 시간을 지정합니다. 정확한 문법은 다음과 같습니다.
>
> | 특수 문자 | 의미 |
> |--|--|
> | * | 모든 시각 |
> | - | 범위 |
> | , | 특정 시간 |
> | / | 증가량 |
>
> Cron 표현식의 사용 예시는 다음과 같습니다.
>
> `0 10 * * *`: 매일 10시 0분에 실행<br>
> `0/20 15 * * *`: 매일 15시 0분부터 20분 간격으로 실행, 즉 15시 0분, 20분, 40분에 실행<br>
> `0 12-15 * * *`: 매일 12시, 13시, 14시, 15시 0분에 실행<br>
> `0 0 15 6,7,8 *`: 6월 7월 8월 15일 0시 0분에 실행<br>

<br>

> [주의]
> 예약 작업의 시작 시간은 현재 시간 기준으로 3분 뒤 이후로만 지정할 수 있습니다. 스케일링 그룹이 변경 중이라면 예약 작업의 실행은 지연될 수 있습니다.

### 생성한 인스턴스 목록 보기
스케일링 그룹 목록에서 원하는 스케일링 그룹을 선택하여 생성한 인스턴스 목록을 확인합니다.

> [주의]
> 스케일링 그룹이 생성한 인스턴스들은 인스턴스 상품의 목록에도 표시됩니다. 그러나 사용자가 임의로 조작할 수는 없습니다.

### 통계 그래프 보기
스케일링 그룹 목록에서 원하는 스케일링 그룹을 선택하여 통계 그래프를 확인합니다.

> [참고]
> 통계 그래프를 통해 스케일링 그룹에 속한 인스턴스들의 최근 1일간 평균 사용량을 확인할 수 있습니다. 또한 증설 또는 감축 시점과 원인을 파악할 수 있습니다.

통계 그래프는 다음과 같은 시스템 자원에 대한 통계 데이터를 제공합니다.

| 시스템 자원 | 제공하는 통계 데이터 |
| --- | --- |
| CPU 사용률 | 스케일링 그룹에 속한 모든 인스턴스의 CPU 사용량의 평균 |
| 메모리 사용률 | 스케일링 그룹에 속한 모든 인스턴스의 메모리 사용량의 평균 |
| 디스크 전송률 | 스케일링 그룹에 속한 모든 인스턴스의 분당 디스크 읽기/쓰기 데이터양의 평균 |
| 네트워크 전송률 | 스케일링 그룹에 속한 모든 인스턴스의 분당 네트워크 송신/수신 데이터양의 평균 |

각 그래프는 5분 간격까지 확대해 볼 수 있으며 최소 1분 단위의 통계 데이터를 제공합니다.

> [참고]
> 통계치를 얻기 위해서는 스케일링 그룹에 속한 각 인스턴스의 데이터를 수집해야 하는데, 모든 인스턴스의 데이터를 동시에 수집하지는 못합니다. 때문에 최근 몇 분간은 일부 인스턴스의 데이터가 없을 수도 있습니다. 통계치는 수집된 데이터가 없는 인스턴스를 제외하고 계산됩니다. 따라서 나중에 누락된 인스턴스의 데이터가 수집되면 값이 변경될 수 있습니다. 모든 인스턴스의 데이터가 수집된 이후에는 항상 동일한 값이 제공됩니다.
