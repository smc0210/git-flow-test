#CMS

##구현 시 고려 사항
1. 현존하는 CMS (워드프레스, Xpress(구 제로보드), 그누보드…) 벤치 마킹
2. 벤치마킹 자료를 참고하여 작은 규모의 프로젝트에 도입될 수준의 퀄리티
3. 실제 업무에 자주 사용되는 반복 업무들의 자동화
4. PHP로 먼저 제작 후 Java, asp, .net 등으로 전환


## 1.CMS 벤치마킹
#### 1-1. 워드프레스
관리자 확인용 URL : http://gnu.dev-smc.com/adm/
계정 : wp

#### 1-2. Xepress 
관리자 확인용 URL : http://xe.dev-smc.com/admin
계정 : smc0210@naver.com

#### 1-3. 그누보드
관리자 확인용 URL : http://gnu.dev-smc.com/adm/
계정 : admin

#### 1-4. Xepress 실제 설치 셋팅
관리자 확인용 URL : http://dev-smc.com/xe/
DB명 : xeTest


## 2. 실제 구현 설계 시나리오
> 기존 CMS 들의 공통점을 통해 알아보는 기초 설계 방안

해당 CMS의 초기파일 `ex)index.php`를 실행하면
가장 먼저 CMS 자동화에 필요한 `권한등을 체크`한 후 `DB 종류 선택`
`DB 정보`등을 입력하고 이를 토대로 기본 골격이 되는 테이블들을 셋팅해 준다.

이를 참고하여 매 프로젝트마다 반복적으로 셋팅이 필요한
`관리자 로그인 페이지` 및 `관리자 계정`등에 필요한 기초 테이블등을 생성해주고,
설정한 값에따라 그 테이블에 데이터를 자동으로 넣어주며(예를 들면 admin/1234),
이에 필요한 `PHP` 혹은 `HTML`페이지를 자동으로 생성해준다.

> 자동 생성되는 HTML 페이지의 예
> ajax로 로그인 체크하는 스크립트

```xml
 <script>
		$(function(){
			$("#admin_id").focus();

			// 아이디, 비밀번호 입력 엔터버튼
			$('#admin_id, #admin_password').keypress(function(e) {
				if (e.keyCode==13) {
					loginProc();
				}
			});
		});

		// 로그인
		function loginProc() {
			form = new $Form('gbForm');
			form.require('admin_id', '아이디');
			form.require('admin_password', '비밀번호');
			if (form.validate()) {
				$.ajax({
					type : "post",
					url : "<?=SITE_URL?>mng/index/loginCheck",
					data : {
						admin_id				:	$("#admin_id").val(),
						admin_password		:	$("#admin_password").val()
					},
					success : function(result){
						if(result == "loginFail") {
							alert("아이디와 비밀번호를 다시 입력해주세요");
						} else {
							$("#loginCheck").val(result);
							alert("관리자로 로그인 되었습니다.");
							form.submit();
						}
					},error : function(result) {

					}
				});
			}
		}
	</script>
```

> 자동 생성되는 PHP(개발소스) 파일 예

```xml
// 페이징 라이브러리
PageNavigation.php
// 함수모음
bs_html_helper.php
bs_message_helper.php
... 생략

```

**여기까지가 도입시에 실제로 반복 케이스를 줄여 업무속도 향상에 도움이 되는 내용이었다면,**

조금더 확장을 해서 기본 게시판을 생성해주는 안을 생각해 볼 수 있는데
현 케이스에서는 사실상 이 게시판 부분을 **주요 쟁점**으로 다뤄야 할 것 같다.

## 3. 실제 구현 설계 시나리오

#### 기본 게시판의 상정 범위 ??





