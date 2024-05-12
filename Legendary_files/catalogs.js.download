var catalogs, scrollBarWidth;
document.addEventListener("DOMContentLoaded", function(event) 
{

    catalogs = document.querySelectorAll('.catalog-wrapper');
		if(!catalogs) return;

		scrollBarWidth = getScrollbarWidth();

		[].forEach.call(catalogs, function(c) {
			sizeCatalog(c);
			//buildCarousel(c);
		});
	
});

window.addEventListener('resize', function(event){
	if(!catalogs) return;
	scrollBarWidth = getScrollbarWidth();
	[].forEach.call(catalogs, function(c) {
		sizeCatalog(c);
	});
});

function sizeCatalog(c) {
	console.group('sizeCatalogs');
	var carousel = c.querySelector('.feed-catalog');
	var carouselScroll = carousel.querySelector('.catalog-scroll');
	var blocks = carousel.getElementsByClassName("preview-block");
	var wrapper = document.getElementById("grid-coverflow-wrapper");
	var wrapperWidth = wrapper.offsetWidth

	//get visible area width
	var c_width = (c.scrollWidth - scrollBarWidth - 1);
	console.log('scrollBarWidth', scrollBarWidth);
	console.log('width', c_width);
	var c_posts_display = parseInt(c.dataset.posts_display);
	var block_width = (c_width / c_posts_display);
	console.log('c_posts_display', c_posts_display);
	console.log('block_width', block_width);

	

	if(wrapperWidth <= 648) {
		// console.log('mobile!');
		block_width = c_width - c.dataset.mobile_padding;
		isMobile = true;
	}  else if(wrapperWidth <= 1024 && wrapperWidth > 648) {
		console.log(c.dataset.mobile_padding);
		block_width = (c_width / 2);
	} else {
		isMobile = false;
	}

	for (var j = 0; j < blocks.length; j++) {
		blocks[j].style.width=(block_width+"px");
	}	
	console.groupEnd();
/**
	var style = blocks[0].currentStyle || window.getComputedStyle(blocks[0]),
	width = blocks[0].offsetWidth, // or use style.width
	margin = parseFloat(style.marginLeft) + parseFloat(style.marginRight),
	padding = parseFloat(style.paddingLeft) + parseFloat(style.paddingRight),
	border = parseFloat(style.borderLeftWidth) + parseFloat(style.borderRightWidth);

	var amount = blocks.length;

	// get the width of the container
	var carouselOffset = parseInt(window.getComputedStyle(carousel.parentNode).width, 10);
	blockWidth = parseInt(width + margin, 10);

	// calcuate the width of the carousel
	carouselScroll.style.width = (amount * carouselOffset) + 'px';
 */
	// carouselScroll.style.transform = 'translateX('+ -blockWidth +'px)';	
	
}
