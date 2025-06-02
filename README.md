# ⚙ 반응형 웹 UI 프로젝트
<image width= "100%" src="mdimg/marymond_nav.png"></image>

HTML, CSS, JavaScript, jQuery를 활용한 반응형 웹 UI 프로젝트입니다.

사용자에게 직관적인 네비게이션, 동적 콘텐츠 로딩, 슬라이드 쇼 및 비디오 플레이어 기능을 제공합니다.


## 📌 주요기능
- 반응형 메뉴 (모바일/데스크탑 구분)
- 메인 슬라이더 (Owl Carousel 적용)
- 상품 슬라이드 (브레이크포인트 설정)
- 인터랙티브 비디오 플레이어
- 탭 메뉴 (HTML/CSS 기반)
  


## 🛠 사용 기술
| 기술 | 설명 |
| --- | --- |
| ![HTML](https://img.shields.io/badge/-HTML-F05032?style=flat-square&logo=html5&logoColor=ffffff) | HTML5 마크업 구조 |
| ![CSS](https://img.shields.io/badge/CSS3-1572B6?style=for-the-square&logo=css3&logoColor=white) | CSS3 반응형 스타일 처리 |
| ![javascript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-square&logo=javascript&logoColor=white) | JavaScript DOM 제어, jQuery 및 Owl Carousel 연동 |
| ![gsap](https://img.shields.io/badge/GSAP-88CE02?style=for-the-square&logo=greensock&logoColor=white) | GSAP 고급 스크롤 애니메이션


## 🧩 기능 상세 설명

### ✅ 반응형 메뉴 구현
- desktopFlag를 기준으로 모바일과 데스크탑을 구분합니다.
- 메뉴가 열려 있으면 닫고, 서브 메뉴도 초기화합니다.
- 초기 실행 시와 resize 이벤트 발생 시 호출됩니다.

```js
	$(window).resize(function(){
		w=$(this).width();

		if(w > 760){
			if($("body").hasClass("prevent")){
				$(".dim").trigger("click");
			}
		}
		if(w <= 920){
			h=200;
		}
		else if(w <= 1070){
			h=290;
		}
		else{
			h=376;
		}
	})

	$(window).trigger("resize");

	// GNB
	$(".gnb_wrap > ul > li").mouseenter(function(){
		$(this).find(".sub").stop().css({ display: "block", height: 0 }).animate({ height: h }, 400);
	});
	$(".gnb_wrap > ul > li").mouseleave(function(){
		$(this).find(".sub").stop().animate({ height: 0 }, 400, function(){
			$(this).css({ display: "none" });
		});
	});
	$(".gnb_wrap > ul > li > a").focusin(function(){
		$(this).parent().addClass("active");
		$(".gnb_wrap .sub").css({ display: "none", height: 0 });
		$(this).next(".sub").css({ display: "block", height: h });
	});
	$(".gnb_wrap .brand a").focusout(function(){
		$(this).parents(".sub").css({ display: "none", height: 0 });
		$(this).parents(".sub").parent().removeClass("active");
	});
	$(".tab").click(function(e){
		e.preventDefault();
		$("body").addClass("prevent");
		$("#mobile").addClass("active");
		$(".dim").addClass("active");
	});
	$(".dim").click(function(){
		$("body").removeClass("prevent");
		$("#mobile").removeClass("active");
		$(".dim").removeClass("active");

		if($(".mobile_inner > ul > li").hasClass("active") == true){
			$(".mobile_inner > ul > li").removeClass("active");
			$(".mobile_inner .sub").removeClass("active");
		}
	});
	$("#mobile .mobile_inner > ul > li").click(function(e){
		e.preventDefault();
		if($(this).hasClass("active") == false){
			$("#mobile .mobile_inner > ul > li").removeClass("active");
			$(this).addClass("active");
			$(".sub").removeClass("active");
			$(this).find(".sub").addClass("active");
		}else{
			$(this).removeClass("active");
			$(this).find(".sub").removeClass("active");
		}
	});

	// Slim Scroll
	$(".mobile_inner").slimscroll({
		height: "100%"
	});
```
<image width= "100%" height= "50%" src="mdimg/marymond_tab.png"></image>


### ✅ 메인 Swiper

- Swiper 라이브러리 통해 메인 슬라이더 구현
- 루프 기능으로 슬라이드 순환
- 자동 재생 설정으로 시간 간격에 따라 슬라이드 전환
- 페이지네이션과 내비게이션 버튼 제공으로 사용자 상호작용 지원

```js
	// Owl Carousel
	let $owl1=$("#carousel1");
	let pageIndex=0;
	$(".main_roll_nav li").eq(pageIndex).addClass("active");

	$owl1.owlCarousel({
		margin: 1,
		autoplay: true,
		loop: true,
		responsive: {
			0: {
				items: 1
			}
		},
		onChanged: callback
	});
	function callback(e){

		pageIndex=e.page.index;

		if(pageIndex == -1) return;

		$(".main_roll_nav li").removeClass("active");
		$(".main_roll_nav li").eq(pageIndex).addClass("active");
	}
	$(".main_roll_nav li").click(function(e){
		e.preventDefault();
		pageIndex=$(this).index();
		$owl1.find(".owl-dot").eq(pageIndex).trigger("click");
	});
```
<image width= "100%" height= "50%" src="mdimg/marymond_slider.png"></image>


### ✅ 다중 Swiepr

- Swiper 라이브러리 통해 상품 슬라이드 구현
- 기본적으로 4개의 슬라이드 표시, 슬라이드 간 간격 설정
- 자동 재생 설정으로 일정 시간 간격에 따라 슬라이드 전환
- 브레이크포인트 설정으로 화면 크기에 따라 슬라이드 수 조정

```js
	let $owl2=$("#carousel2");

	$owl2.owlCarousel({
		margin: 1,
		autoplay: true,
		loop: true,
		responsive: {
			0: {
				items: 1
			},
			767: {
				items: 2
			}
		}
	});
```

### ✅ 인터랙티브 비디오 플레이어

- 사용자가 비디오를 재생하고 일시정지할 수 있는 기능 제공
- 비디오 재생이 완료되면 자동으로 초기화 다시 시청 가능하도록 준비
```js
	// Video
	let video=document.getElementById("my_video");

	$("#toggle").click(function(e){
		e.preventDefault();
		video.play();
		$(this).fadeOut(300);
	});
	$("#my_video").click(function(){
		video.pause();
		$('#toggle').fadeIn(300);
	});
	video.addEventListener("ended", function(){
		$("#toggle").fadeIn(300);
		video.pause();
		video.currentTime=0;
	});
```




