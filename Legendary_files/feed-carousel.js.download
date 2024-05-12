var carousels, blocks;
var blockWidth = 0;
var isMobile = false;


document.addEventListener("DOMContentLoaded", function(event)
{

    // carousels = document.querySelectorAll('.feed-wrapper');
		// if(!carousels) return;

		// [].forEach.call(carousels, function(c) {
		// 	sizeCarousels(c);
		// 	buildCarousel(c);
		// });
		// window.addEventListener('resize', function(event){
		// 	[].forEach.call(carousels, function(c) {
		// 		sizeCarousels(c);
		// 	});
		// });
});



// function sizeCarousels(c) {
// 	var carousel = c.querySelector('.feed-carousel');
// 	var carouselScroll = carousel.querySelector('.feed-scroll');
// 	blocks = carousel.getElementsByClassName("preview-block");
// 	var docWidth = document.body.clientWidth;
// 	//get visible area width
// 	var c_width = c.offsetWidth;
// 	var c_posts_display = parseInt(c.dataset.posts_display);
// 	var block_width = (c_width / c_posts_display);

//   if (carousel.classList.contains('trailer')) {
//     return;
//   }


// 	if(docWidth <= 640) {
// 		// console.log('mobile!');
// 		block_width = c_width - c.dataset.mobile_padding;
// 		isMobile = true;
// 	}  else {
// 		isMobile = false;
// 	}

// 	for (var j = 0; j < blocks.length; j++) {
// 		blocks[j].style.width=(block_width+"px");
// 	}

// 	var style = blocks[0].currentStyle || window.getComputedStyle(blocks[0]),
// 	width = blocks[0].offsetWidth, // or use style.width
// 	margin = parseFloat(style.marginLeft) + parseFloat(style.marginRight),
// 	padding = parseFloat(style.paddingLeft) + parseFloat(style.paddingRight),
// 	border = parseFloat(style.borderLeftWidth) + parseFloat(style.borderRightWidth);

// 	var amount = blocks.length;

// 	// get the width of the container
// 	var carouselOffset = parseInt(window.getComputedStyle(carousel.parentNode).width, 10);
// 	blockWidth = parseInt(width + margin, 10);

// 	// calcuate the width of the carousel
// 	carouselScroll.style.width = (amount * carouselOffset) + 'px';

// 	// carouselScroll.style.transform = 'translateX('+ -blockWidth +'px)';

// }

// function buildCarousel(c) {

// 	var i = 0;
// 	var carousel = c.querySelector('.feed-carousel');
// 	var rightArrow = c.querySelector('.right-arrow');
// 	var leftArrow = c.querySelector('.left-arrow');
// 	var c_posts_display = parseInt(c.dataset.posts_display);
// 	var carouselScroll = carousel.querySelector('.feed-scroll');
// 	blocks = carousel.getElementsByClassName("preview-block");

// 	var amount = blocks.length;
//   console.log(carousel);
// 	c.style.opacity = 1;

// 	// add click events to control arrows
// 	// document.getElementById('prev').addEventListener('click', prev, true);
// 	if(rightArrow && leftArrow) {
// 		rightArrow.addEventListener('click', function(e){

// 			var thisBlock = e.target.parentNode.querySelector('.preview-block');
// 			i++;
// 			var stopScollNum = amount - i;

// 			carouselScroll.style.transform = 'translateX('+ -thisBlock.offsetWidth * i +'px)';
// 			if(i > 0) {
// 				leftArrow.classList.add('active');
// 			} else {
// 				leftArrow.classList.remove('active');
// 			}
// 			if(stopScollNum <= c_posts_display) {
// 				rightArrow.classList.remove('active');
// 			} else {
// 				rightArrow.classList.add('active');
// 			}
// 		}, true);

// 		leftArrow.addEventListener('click', function(e){
// 			var thisBlock = e.target.parentNode.querySelector('.preview-block');
// 			i--;
// 			var stopScollNum = amount - i;

// 			carouselScroll.style.transform = 'translateX('+ -thisBlock.offsetWidth * i +'px)';
// 			if(i < 1) {
// 				leftArrow.classList.remove('active');
// 			} else {
// 				leftArrow.classList.add('active');
// 			}
// 			if(stopScollNum <= c_posts_display) {
// 				rightArrow.classList.remove('active');
// 			} else {
// 				rightArrow.classList.add('active');
// 			}

// 		}, true);
// 	}
// 	detectswipe(carousel ,function (el,direc){
// 		var thisBlock = el.querySelector('.preview-block');
// 		if(direc == "l") i++;
// 		else if(direc == "r") i--;

// 		if(i < amount && i >= 1) {
// 		} else if (i <= 0) {
// 			i = 0;
// 		} else {
// 			i = amount - 1;
// 		}
// 		carouselScroll.style.transform = 'translateX('+ -thisBlock.offsetWidth * i +'px)';
// 		updateDots(i, el);
// 	});
// 	// setInterval(function(){
// 	// 	next();
// 	// }, 4000);
// }

// function updateDots(i, el) {
// 	var dotsWrapper = el.querySelector('.carousel-dot-wrapper');
// 	if(dotsWrapper) {
// 		var dots = dotsWrapper.querySelectorAll('a');
// 		for (let d = 0; d < dots.length; d++) {
// 			dots[d].classList.remove('active');
// 		}
// 		dots[i].classList.add('active');
// 	}

// }



$(document).ready(function(){

});
