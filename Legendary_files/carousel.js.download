var moveIndex = 1,
    slidesLength = 0,
		slideWidth = 0,
    currTransl = [],
    translationComplete = true,
    moveOffset = 0,
		margin = 41,
		heroCarousel,
		videoSlides,
		videoSlideParents,
		carouselSlides,
		slideSpin,
		allowShift = true,
		winWidth;

document.addEventListener("DOMContentLoaded", function(event) {
		let heroWrapper = document.getElementById('hero_wrapper');
		heroCarousel = document.getElementById('carousel');

		if(!heroCarousel) return;

    console.log('HERO WRAPPER: ', heroWrapper);
    slidesLength = document.getElementsByClassName("slide").length;

		var slides = heroCarousel.getElementsByClassName('slide'),
		firstSlide = slides[0],
		secondSlide = slides[1],
		lastSlide = slides[slidesLength - 1],
		cloneFirst = firstSlide.cloneNode(true),
		cloneSecond = secondSlide.cloneNode(true),
		cloneLast = lastSlide.cloneNode(true);

		heroCarousel.appendChild(cloneFirst);
		heroCarousel.appendChild(cloneSecond);
		heroCarousel.insertBefore(cloneLast, firstSlide);

		videoSlideParents = heroCarousel.querySelectorAll(".with-video");
		videoSlides = heroCarousel.querySelectorAll("video");
		carouselSlides = heroCarousel.querySelectorAll(".slide");
		winWidth = window.innerWidth;

		var rightArrow = heroWrapper.querySelector('.right-arrow');
		var leftArrow = heroWrapper.querySelector('.left-arrow');

		if(rightArrow && leftArrow) {
			rightArrow.addEventListener('click', function(e){
				let slideNum = getActiveNum();
				slideNum = slideNum + 1;
				goToSlide(slideNum);
			}, true);

			leftArrow.addEventListener('click', function(e){
				let slideNum = getActiveNum();
				slideNum = slideNum - 1;
				// if(slideNum == -1) slideNum = slidesLength - 1;
				goToSlide(slideNum);

			}, true);
		}

		sizeHeroCarousel();

		if(videoSlides.length > 0){
			[].forEach.call(videoSlides, function(slideVideo) {
				slideVideo.pause();
				slideVideo.onended = (e) => {
						next();
					}
			});
		}
		checkIndex();

		heroCarousel.addEventListener('transitionend', checkIndex);

		$('#home-dots a').click(function(){
			var dot_index = $(this).data('dot_count');
			$('#home-dots a').removeClass('current-slide');
			$(this).addClass('current-slide');
			goToSlide(dot_index);
		});

		detectswipe(heroCarousel ,function (el,direc){
			let slideNum = getActiveNum();
			if(direc == "l") slideNum = slideNum + 1;
			else if(direc == "r") slideNum = slideNum - 1;
			// if(slideNum == -1) slideNum = slidesLength - 1;
			goToSlide(slideNum);

		});

		if(videoSlides.length > 0) {
			$('.resize-overlay').show();
			videoSlides[0].load();
			videoSlides[0].oncanplay = (event) => {
				$('.resize-overlay').hide();
				spinCarousel();
			};
		} else {
			spinCarousel();
		}

	var timeOutFunctionId;
	window.addEventListener('resize', function(event){
		if (window.innerWidth != winWidth) {
			winWidth = window.innerWidth;
			heroCarousel.classList.remove('shifting');
			sizeHeroCarousel();
			let activeIndex = getActiveNum();
			heroCarousel.style.transform = 'translateX('+ -slideWidth * activeIndex +'px)';
			clearTimeout(timeOutFunctionId);
			timeOutFunctionId = setTimeout(resized, 500);
		}
	});
});

function resized() {
	heroCarousel.classList.add('shifting');
	let activeIndex = getActiveNum();
	heroCarousel.style.transform = 'translateX('+ -slideWidth * activeIndex +'px)';
	spinCarousel();
}

function getActiveNum() {
	let activeNum;
	[].forEach.call(carouselSlides, function(cSlide) {
		let curSlide = cSlide.classList.contains('current-slide');
		if(curSlide) activeNum = cSlide;
	});
	return parseInt(activeNum.dataset.slide_count);
}

function spinCarousel() {
	var hasVideo = carouselSlides[moveIndex].classList.contains('with-video');
	carouselSlides[moveIndex].classList.add('current-slide');
	clearInterval(slideSpin);
	if(hasVideo) {
		carouselSlides[moveIndex].querySelector('video').play();
	} else {
		slideSpin = setInterval(function(){
			next();
		}, 4000);
	}
}
function vw(v) {
  var w = Math.max(document.documentElement.clientWidth, window.innerWidth || 0);
  return (v * w) / 100;
}

function sizeHeroCarousel() {
	var slides = heroCarousel.getElementsByClassName("slide"),
			slidesLength = slides.length,
			style = slides[0].currentStyle || window.getComputedStyle(slides[0]),
			width = slides[0].offsetWidth, // or use style.width
			margin = parseFloat(style.paddingLeft) + parseFloat(style.paddingRight);

			let thisVw = vw(90);
			if(winWidth >= 1450) {
				thisVw = 1280;
			} else if (winWidth < 648 && winWidth >= 420) {
				thisVw = style.width;
			} else if (winWidth < 420) {
				thisVw = vw(100);
			}

			heroCarousel.style.width = (thisVw * (slidesLength+1)) + 'px';
			slideWidth = thisVw;
}

function pauseVideos() {
	if(videoSlides.length > 0){
		[].forEach.call(videoSlides, function(slideVideo) {
			slideVideo.pause();
		});
	}
}



function prev()
{
	clearInterval(slideSpin);
	pauseVideos();
	heroCarousel.classList.add('shifting');
	// if (allowShift) {
		heroCarousel.style.transform = 'translateX('+ -slideWidth * moveIndex +'px)';
		moveIndex--;
	// }
	allowShift = false;
}

function next()
{
	clearInterval(slideSpin);
	pauseVideos();
	heroCarousel.classList.add('shifting');
	// if (allowShift) {
		heroCarousel.style.transform = 'translateX('+ -slideWidth * moveIndex +'px)';
		moveIndex++;
	// }
	allowShift = false;
}

function goToSlide(nextIndex) {
	var offsetMultiply = nextIndex - moveIndex
	moveIndex = moveIndex + offsetMultiply;
	next();
}

function checkIndex(){
	heroCarousel.classList.remove('shifting');
	if (moveIndex == -1) {
		heroCarousel.style.transform = 'translateX('+  -(slidesLength * slideWidth) +'px)';
		moveIndex = slidesLength - 1;
	}

	if (moveIndex == 0) {
		heroCarousel.style.transform = 'translateX('+  -((slidesLength - 1) * slideWidth) +'px)';
		moveIndex = slidesLength;
	}

	if (moveIndex == slidesLength + 1) {
		heroCarousel.style.transform = 'translateX(0px)';
		moveIndex = 1;
	}
	allowShift = true;

	updateNavDots();
	spinCarousel();
}

function updateNavDots() {
	var slideDots = document.getElementById('home-dots');
	if(!slideDots) return;
	var dots = slideDots.querySelectorAll(".dot");
	[].forEach.call(dots, function(dot) {
		dot.classList.remove("current-slide");
	});

	// console.log('moveIndex',moveIndex);
	let activeDot = moveIndex;
	if(activeDot == -1 || activeDot == 0) activeDot = slidesLength;
	// console.log('activeDot',activeDot);


	slideDots.querySelector('[data-dot_index="'+ activeDot +'"]' ).classList.add('current-slide');
	[].forEach.call(dots, function(dot) {
		dot.classList.remove("current-slide");
	});
	slideDots.querySelector('[data-dot_index="'+ activeDot +'"]' ).classList.add('current-slide');

	[].forEach.call(carouselSlides, function(activeSlide) {
		activeSlide.classList.remove("current-slide");
	});

}

function transition_titles_out() {
	$('.hero-item-content').fadeOut(100).css('bottom','30px').bind("transitionend webkitTransitionEnd oTransitionEnd MSTransitionEnd", function(e) {
			e.stopPropagation();
	});
}

function transition_titles_in() {
	var bottom_px = '50px';
	if($('#hero_wrapper').hasClass('optin-open')){
			bottom_px = '90px';
	}
	$('.hero-item-content').fadeIn(600).css('bottom',bottom_px).bind("transitionend webkitTransitionEnd oTransitionEnd MSTransitionEnd", function(e) {
			e.stopPropagation();
	});
}

$(document).ready(function(){
	transition_titles_in();
	// $('#hero_wrapper').bind("transitionend webkitTransitionEnd oTransitionEnd MSTransitionEnd", function() {
	// 	transition_titles_in();

	// 	// if we are not on the first slide find the hero video player and change it's URL to both stop it and remove the autoplay parameter
	// 	if( moveIndex != 1 && $('.youtube-hero').length ) {
	// 			if( player.getPlayerState() == YT.PlayerState.PLAYING ) {
	// 					player.pauseVideo();
	// 			}
	// 	}

	// 	setTimeout(
	// 			function() {
	// 					animation_complete = true;
	// 	}, 1000);

	// });
});
