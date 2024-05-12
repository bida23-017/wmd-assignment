$(document).ready(function(){
	// Setup the player while preventing YT API and Doc Ready race.
	var galleryPlayer, isPlayerReady = false;
	function openGallery(video_container) {
		if(isPlayerReady) {
			galleryPlayer.loadVideoById(video_container.attr('data-youtube_id'));
			$('.video-gallery').fadeIn(600, function(){
				gallery_video_scale();
			});		
	
			return;
		}
		if (isYoutubeReady && isDocReady) {
			galleryPlayer = new YT.Player('gallery-youtube-iframe', {
						videoId: video_container.attr('data-youtube_id'),
						width: video_container.attr('data-youtube_width'),
						height: video_container.attr('data-youtube_height'),
						playerVars: {
								rel: ( video_container.attr('data-youtube_rel') ? video_container.attr('data-youtube_rel') : 0 ),
								hd: ( video_container.attr('data-youtube_hd') ? video_container.attr('data-youtube_hd') : 1 ),
								autohide: ( video_container.attr('data-youtube_autohide') ? video_container.attr('data-autohide') : 1 ),
								showinfo: ( video_container.attr('data-youtube_showinfo') ? video_container.attr('data-youtube_showinfo') : 0 ),
								autoplay: ( video_container.attr('data-youtube_autoplay') ? video_container.attr('data-youtube_autoplay') : 0 ),
								controls: ( video_container.attr('data-youtube_controls') ? video_container.attr('data-youtube_controls') : 1 ),
								wmode: 'opaque'
						},
						events: {
								'onReady': onGalleryPlayerReady,
								'onStateChange': onGalleryPlayerStateChange
						}
				});
		}
	}

	function onGalleryPlayerReady(event) {
		isPlayerReady = true;
		$('.video-gallery').fadeIn(600, function(){
			gallery_video_scale();
			galleryPlayer.playVideo();
		});		
	}

	function onGalleryPlayerStateChange(event) {
		
	}

	function gallery_video_scale(){
		if($('.video-gallery iframe').length){
				$('.video-gallery').each(function(){
					var width = $('iframe', this).width();
					var height =  (width / 16) * 9;
					$('iframe', this).height(height);
				});
		}
	}

	//gallery video play
	$('.video-poster').click(function(){
		if($(this).hasClass('youtube-gallery')){
			openGallery($(this))
		}
	});
	$('.video-gallery').click(function(e){
		e.stopPropagation();
		galleryPlayer.stopVideo();

		$(this).fadeOut(600, function(){
				// if($('#gallery-youtube-iframe', this).length){
				// 		$('#gallery-youtube-iframe', this).remove();
				// }
				
				// galleryPlayer = null;
		});

	});	
});

