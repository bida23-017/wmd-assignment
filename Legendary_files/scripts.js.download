// define $
$ = jQuery.noConflict();

function setCookie(name, value, days) {
  var expires = "";
  if (days) {
    var date = new Date();
    date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
    expires = "; expires=" + date.toUTCString();
  }
  document.cookie = name + "=" + (value || "") + expires + "; path=/";
}
function getCookie(name) {
  var nameEQ = name + "=";
  var ca = document.cookie.split(';');
  for (var i = 0; i < ca.length; i++) {
    var c = ca[i];
    while (c.charAt(0) == ' ') c = c.substring(1, c.length);
    if (c.indexOf(nameEQ) == 0) return c.substring(nameEQ.length, c.length);
  }
  return null;
}

function isTouchDevice() {
  return (('ontouchstart' in window) ||
    (navigator.maxTouchPoints > 0) ||
    (navigator.msMaxTouchPoints > 0));
}

// Youtube iframe API Race Condition
var isDocReady = false, isYoutubeReady = false, isYoutubePlayerReady = false;

if (typeof disable_coverflow === 'undefined') {
  disable_coverflow = false;
};

$(document).ready(function () {
  let lastKnownScrollPosition = 0;
  let ticking = false;

  setTimeout(() => {
    const inlineNewsletter = $('#inline_newsletter');
    const cookieCheck = getCookie('inlineNewsletter');

    if (inlineNewsletter.length && !cookieCheck) {
      $.colorbox({
        href: '#inline_newsletter',
        inline: true,
        maxWidth: '90%',
        onComplete: () => {
          const btn = $('#inline_newsletter #mc-embedded-subscribe');

          btn.on('click', () => {
            $.colorbox.close();
          });
        },
        onClosed: () => {
          setCookie('inlineNewsletter', 'closed', 7);
        }
      });
    }
  }, 3000);

  if (isTouchDevice()) {
    document.querySelector('body').classList.add('isTouch');
  }

  document.addEventListener('scroll', function (e) {
    lastKnownScrollPosition = window.scrollY;

    if (!ticking) {
      window.requestAnimationFrame(function () {
        const body = document.querySelector('body');

        if (lastKnownScrollPosition > 0) {
          body.classList.add('is-scrolling');
        } else {
          body.classList.remove('is-scrolling');
        }
        ticking = false;
      });

      ticking = true;
    }
  });

  //vars for loading updates
  var offset_base = $('#load-more-posts-button').data('postoffset');
  var offset = offset_base;

  //vars for responsive resizing
  var compareWidth;
  var detector;
  detector = $('header'); // whatever container accurately reflects the site width
  compareWidth = detector.width();

  // hero_wrapper_scale(16,9);

  // Notification bar
  var $notificationBar = $('.notifications-bar');
  if ($notificationBar.length) {
    var notificationBarCookie = getCookie('notificationBar');
    var $notificationClose = $notificationBar.find('.notifications-bar--close');

    if (notificationBarCookie) {
      $notificationBar.remove();
    } else {
      $notificationBar.addClass('is-visible');
    }

    $notificationClose.on('click', () => {
      $notificationBar.remove();
      setCookie('notificationBar', true, 7);
    });
  }

  //navicon actions
  $('#navicon, #menu-overlay').click(function () {

    //ga tracking
    if ($('#navicon').hasClass('open')) {
      ga_event('Main Nav', 'close', '');
    } else {
      ga_event('Main Nav', 'open', '');
    }

    $('#navicon').toggleClass('open');
    $('#main-menu').toggleClass('open');
    $('#menu-overlay').fadeToggle();
    $('body').toggleClass('menu-open');
  });

  //taxonomy shelf show/hide
  $('header .shelf-button').click(function () {
    $(this).toggleClass('active');
    if (compareWidth > 1023 && !$('.page-template-page_template_updates').length) {
      $(this).next('.shelf-dropdown').slideToggle();
    }

    //ga
    var page_name = $('.first-crumb').text();
    if ($(this).hasClass('active')) {
      var ga_action = 'open';
    } else {
      var ga_action = 'close';
    }
    ga_event('Category Filter', ga_action, page_name);
  });

  //mobile taxonomy shelf drop down
  $('#mobile-taxonomy-shelf .shelf-button').click(function () {
    $(this).toggleClass('active');
    $(this).next('.shelf-dropdown').slideToggle();

    //ga
    var page_name = $('.first-crumb').text();
    if ($(this).hasClass('active')) {
      var ga_action = 'open';
    } else {
      var ga_action = 'close';
    }
    ga_event('Category Filter', ga_action, page_name);
  });

  //toggle properties between grid & coverflow
  $('#properties-toggle a').click(function () {
    if (!$(this).hasClass('active')) {
      clear_all_filters();
      var display_type = $(this).data('display_type');
      base_toggle(this, display_type);
      if (display_type == "cover-flow") {
        initialize_coverflow();
      }
      else {
        destroy_coverflow();
      }

    }
  });

  //taxonomy shelf filtering
  $('.shelf-dropdown a').click(function () {

    if (($('#properties-wrapper').length) && ($('#properties-wrapper').hasClass('cover-flow'))) {
      $('#grid-toggle').trigger('click');
    }
    var filter_name = $(this).data('filter_name');
    $('.shelf-dropdown a').removeClass('active');
    $('.shelf-dropdown a[data-filter_name=' + filter_name + ']').addClass('active');
    var active_filter_text = $(this).data('filter_button_text');
    var shelf_button = $('.shelf-button');
    shelf_button.each(function () {
      $(this).trigger('click').text(active_filter_text);
    });

    if ($('#properties-wrapper').length) {
      if (filter_name == 'all') {
        $('.property-item').show();
      }
      else {
        $('.property-item').each(function () {
          var filters = $(this).data('filter_1');
          if (filters.indexOf(filter_name) == -1) {
            $(this).hide();
          }
          else {
            $(this).show();
          }
        });
      }
    }
    else if ($('#updates-wrapper')) {
      offset = $('#load-more-posts-button').data('postoffset');
      var ajax_path = $('#load-more-posts-button').data('path');
      $('#load-more-posts-button').hide();
      $('#updates-wrapper').html('');
      $('#more-posts').html('');
      if (filter_name == 'all') {
        $('#more-updates-wrapper .loading').fadeIn(400, function () {
          $('#updates-wrapper').load(ajax_path, function () {
            $('#more-updates-wrapper .loading').slideUp(400);
            $('#load-more-posts-button').show();
            if (compareWidth < 1024) {
              if ($('.update-grid-item').length) {
                updates_grid_images('responsive');
              }
            }
            if (compareWidth > 1024) {
              if ($('.update-grid-item').length) {
                updates_grid_images('desktop');
              }
            }
          });
        });
      }
      else {
        $('#more-updates-wrapper .loading').fadeIn(400, function () {
          $('#updates-wrapper').load(ajax_path + '?ajax=true&filter=' + filter_name, function () {
            $('#more-updates-wrapper .loading').slideUp(400);
            if (compareWidth < 1024) {
              if ($('.update-grid-item').length) {
                updates_grid_images('responsive');
              }
            }
            if (compareWidth > 1024) {
              if ($('.update-grid-item').length) {
                updates_grid_images('desktop');
              }
            }
          });
        });
      }
    }

    //ga
    var page_name = $('.first-crumb').text();
    ga_event('Category Filter', 'filter click', 'page: ' + page_name + ' | filter: ' + filter_name);
  });

  //coverflow filtering
  $('#cover-flow-filters a').click(function () {
    var filter_name = $(this).data('release_type');
    $('#cover-flow-filters a').removeClass('active');
    $('#cover-flow-filters a[data-release_type=' + filter_name + ']').addClass('active');
    if (filter_name == 'all') {
      $('.property-item').show().removeClass('hidden');
    }
    else {
      $('.property-item').each(function () {
        var filters = $(this).data('filter_2');
        if (filters.indexOf(filter_name) == -1) {
          $(this).hide().addClass('hidden');
        }
        else {
          $(this).show().removeClass('hidden');
        }
      });
    }
    refresh_coverflow();
  });

  //calculate various sizes on page load
  taxonomy_shelf_sizing();
  //crumb_2_sizing();

  home_item_height();
  //smartquest_title_crop();

  //responsive resize stuff

  //drop in images based on browser size on page load
  if (compareWidth <= 479) {
    resp_img_swap('mobile');
    hero_img_swap('mobile');
  }
  else if (compareWidth > 479 && compareWidth < 769) {
    resp_img_swap('tablet');
    hero_img_swap('tablet');
  }
  else if (compareWidth > 769) {
    resp_img_swap('desktop');
    hero_img_swap('desktop');
  }
  if (compareWidth < 1024) {
    if ($('.update-grid-item').length) {
      updates_grid_images('responsive');
    }
  }
  if (compareWidth > 1023) {
    if ($('.update-grid-item').length) {
      updates_grid_images('desktop');
    }
  }
  if (compareWidth > 1023) {
    flex_row_height();
  }

  $(window).resize(function () {
    home_item_height();
    //home_dots_position();
    //hero_video_scale();
    video_scale();
    //smartquest_title_crop();

    if ($('.flex-gallery').is(':visible')) {
      gallery_resize();
    }

    if (compareWidth <= 479) {

    }
    else if (compareWidth > 479 && compareWidth < 769) {

    }
    else if (compareWidth > 769) {

    }
    if (compareWidth > 1023) {
      flex_row_height();
    }

    if (detector.width() !== compareWidth) {
      //resize taxonomy shelf width
      taxonomy_shelf_sizing();
      //crumb_2_sizing();

      //size changes from desktop to anything smaller
      if (compareWidth > 1024 && detector.width() < 1025) {
        if ($("#properties-list").data("coverflow")) {
          destroy_coverflow();
        }
        $('#grid-toggle').trigger('click');
      }


      //size changes from desktop to tablet portrait
      if (compareWidth >= 769 && detector.width() < 769 && detector.width() > 479) {
        resp_img_swap('tablet');
        hero_img_swap('tablet');
        Waypoint.disableAll();
        $('.flexible-content-row').attr('style', '');
      }
      //size changes from desktop to mobile portrait
      if (compareWidth >= 769 && detector.width() <= 479) {
        resp_img_swap('mobile');
        hero_img_swap('mobile');
        Waypoint.disableAll();
        $('.flexible-content-row').attr('style', '');
      }
      //size changes from tablet portrait to desktop
      if (detector.width() >= 769 && compareWidth < 769) {
        resp_img_swap('desktop');
        hero_img_swap('desktop');
        Waypoint.enableAll();
        Waypoint.refreshAll();
      }
      //size changes from tablet portrait to mobile
      if (compareWidth > 479 && detector.width() <= 479) {
        resp_img_swap('mobile');
        hero_img_swap('mobile');
        Waypoint.disableAll();
        $('.flexible-content-row').attr('style', '');
      }
      //size changes from mobile to tablet portrait
      if (detector.width() >= 479 && detector.width() < 769 && compareWidth < 479) {
        resp_img_swap('tablet');
        hero_img_swap('tablet');
        Waypoint.disableAll();
        $('.flexible-content-row').attr('style', '');
      }
      //size changes from mobile to desktop
      if (detector.width() >= 769 && compareWidth < 479) {
        resp_img_swap('desktop');
        hero_img_swap('desktop');
        Waypoint.enableAll();
        Waypoint.refreshAll();
      }
      //size changes from desktop or tablet landscape to anything smaller
      if (compareWidth > 1023 && detector.width() <= 1023) {
        if ($('.update-grid-item').length) {
          updates_grid_images('responsive');
        }
        $('.flexible-content-row').attr('style', '');
      }
      //size changes to desktop or tablet landscape from anything smaller
      if (compareWidth < 1024 && detector.width() >= 1024) {
        if ($('.update-grid-item').length) {
          updates_grid_images('desktop');
        }
        flex_row_height();
      }

      //set new compare width
      compareWidth = detector.width();
    }
  });

  taxonomy_shelf_waypoint;

  //updates load more posts

  $('#load-more-posts-button').click(function () {
    var totalposts = $(this).data('totalposts');
    var ajax_path = $(this).data('path');

    //ga
    ga_event('Latest News', 'load more', 'offset: ' + offset);

    $('#more-updates-wrapper .loading').slideDown(400, function () {
      $('#more-posts').append('<div class="posts-' + offset + '"></div>');
      $('.posts-' + offset).load(ajax_path + '?ajax=true&offset=' + offset, function () {
        $('#more-updates-wrapper .loading').slideUp(400);
        if (compareWidth < 1024) {
          if ($('.update-grid-item').length) {
            updates_grid_images('responsive');
          }
        }
        if (compareWidth > 1023) {
          if ($('.update-grid-item').length) {
            updates_grid_images('desktop');
            Waypoint.refreshAll();
          }
        }
      });
      offset = offset + offset_base;
      if (offset >= totalposts) {
        $('#load-more-posts-button').hide();
      }
    });
  });

  //hero video play
  $('#hero-video-wrapper').click(function () {
    if ($(this).hasClass('brightcove-hero')) {

      var brightcove_video = $(this).data('brightcove_id');
      var brightcove_video_string = $(this).data('unique_string');
      $('.hero-video', this).fadeIn(600, function () {
        hero_video_scale();
        videojs("video_" + brightcove_video + "_" + brightcove_video_string).play();
        //setTimeout(function() {videojs("video_"+brightcove_video+"_"+brightcove_video_string).play();},600);
      });

    } else if ($(this).hasClass('youtube-hero')) {
      // autoplaying a video will cause the player to hang on mobile devices
      var mobile = /(iPad|iPhone|iPod|Android)/g.test(navigator.userAgent);
      if (!mobile) {
        player.playVideo();
      }

      $('.hero-video', this).fadeIn(600, function () {
        hero_video_scale();
      });
    }

    //ga
    var page_name = $('h1#item-detail-title').text();
    ga_event('Hero Video', 'open', page_name);
  });
  $('.hero-video').click(function (e) {
    e.stopPropagation();
    $(this).fadeOut(600, function () {
      player.stopVideo();

      if ($('#hero-brightcove-container', this).length) {
        var brightcove_video = $('#hero-video-wrapper').data('brightcove_id');
        var brightcove_video_string = $('#hero-video-wrapper').data('unique_string');
        videojs("video_" + brightcove_video + "_" + brightcove_video_string).pause();
      }
    });

    //ga
    var page_name = $('h1#item-detail-title').text();
    ga_event('Hero Video', 'close', page_name);
  });

  //flex video play
  $('.video-box .flex-video-link').click(function () {
    var video_container = $(this).parent('.video-box').children('.flex-video');

    if ($(this).hasClass('flex-brightcove')) {

      var brightcove_video = $(this).data('brightcove_id');
      var brightcove_video_string = $(this).data('unique_string');

      $(video_container).fadeIn(600, function () {
        video_scale();
        setTimeout(function () { videojs("video_" + brightcove_video + "_" + brightcove_video_string).play(); }, 600);
      });

    } else if ($(this).hasClass('flex-youtube')) {

      var youtube_id = $(this).data('youtube_id');
      var youtube_iframe_src = 'https://www.youtube.com/embed/' + youtube_id + '?rel=0&amp;autoplay=1&amp;showinfo=0';
      $('iframe', video_container).attr('src', youtube_iframe_src);

      $(video_container).fadeIn(600, function () {
        video_scale();
      });
    }

    //ga
    var page_name = $('h1#item-detail-title').text();
    var video_name = $('h2.flex-title', this).text();
    var event_label = page_name + ' | ' + video_name;
    ga_event('Video Module', 'open', event_label);
  });
  $('.flex-video').click(function (e) {
    e.stopPropagation();
    $(this).fadeOut(600, function () {
      if ($('iframe', this).length) {
        $('iframe', this).attr('src', '');
      }
      if ($('.flex-brightcove-container', this).length) {
        var brightcove_video = $(this).parent('.video-box').children('.flex-video-link').data('brightcove_id');
        var brightcove_video_string = $(this).parent('.video-box').children('.flex-video-link').data('unique_string');
        videojs("video_" + brightcove_video + "_" + brightcove_video_string).pause();
      }
    });

    //ga
    var page_name = $('h1#item-detail-title').text();
    var video_name = $('.full-screen-title h2', this).text();
    var event_label = page_name + ' | ' + video_name;
    ga_event('Video Module', 'close', event_label);
  });

  //flex gallery launch
  var start_slide = 0;
  if (QueryString.i && QueryString.t) {
    start_slide = QueryString.i;
  }
  $('.gallery-box a.button').click(function () {
    var active_gallery_wrapper = $(this).closest('.gallery-box').children('.flex-gallery');
    var active_gallery = $('.gallery-inner', active_gallery_wrapper);
    $('li img', active_gallery).each(function () {
      var src = $(this).data('src');
      $(this).attr('src', src);
    });

    //console.log('slide: '+start_slide);
    $(active_gallery).flexslider({
      'directionNav': false,
      'slideshowSpeed': 3000,
      'smoothHeight': false,
      'startAt': start_slide,
      //    'multipleKeyboard': true,
      'start': function (slider) {
        $('.gallery-toggle-wrapper a.gallery_next').click(function (event) {
          event.preventDefault();
          slider.flexAnimate(slider.getTarget("next"));
          slider.flexslider('stop');
        });
        $('.gallery-toggle-wrapper a.gallery_prev').click(function (event) {
          event.preventDefault();
          slider.flexAnimate(slider.getTarget("prev"));
          slider.flexslider('stop');
        });
        gallery_resize();
      },
      'before': function () {
        gallery_resize();
      },
      'after': function () {

      }
    });
    active_gallery_wrapper.fadeIn();
    start_slide = 0;

    //ga
    var page_name = $('h1#item-detail-title').text();
    var gallery_name = $('.full-screen-title h2', active_gallery_wrapper).text();
    var event_label = page_name + ' | ' + gallery_name;
    ga_event('Gallery Module', 'open', event_label);
  });

  $('.flex-gallery').click(function (e) {
    e.stopPropagation();
    $(this).fadeOut(600, function () {
      $('.gallery-inner').flexslider('destroy');
      $('.gallery-image-wrapper').attr('style', '');
      $('ul.slides li').css('height', '');
    });

    //ga
    var page_name = $('h1#item-detail-title').text();
    var gallery_name = $('.full-screen-title h2', this).text();
    var event_label = page_name + ' | ' + gallery_name;
    ga_event('Gallery Module', 'close', event_label);
  });

  $('.flex-gallery').on('click', '.flex-control-nav li', function (e) {
    e.stopPropagation();
  });
  $('.flex-gallery').on('click', '.gallery-inner', function (e) {
    e.stopPropagation();
  });

  $('.gallery-toggle-wrapper').on('click', 'a', function (e) {
    e.stopPropagation();
  });

  $('#hero-brightcove-container').on('click', '.video-js', function (e) {
    e.stopPropagation();
  });

  $('.flex-video').on('click', '.video-js', function (e) {
    e.stopPropagation();
  });

  $('.flex-video').on('click', '.sharing-container', function (e) {
    e.stopPropagation();
  });

  $('.flex-gallery').on('click', '.sharing-container', function (e) {
    e.stopPropagation();
  });

  $('.hero-video').on('click', '.sharing-container', function (e) {
    e.stopPropagation();
  });

  //close homepage optin
  $('.close-optin').click(function () {
    $('#hero_wrapper').removeClass('optin-open');
    $('.hero-item-content').css('bottom', '50px');
  });

  //drop content show/hide
  $('.drop-content h2').click(function () {
    $(this).toggleClass('active');
    $(this).next('.drop_content_shelf_wrapper').slideToggle();

    //ga
    var page_name = $('h1#item-detail-title').text();
    if ($(this).hasClass('active')) {
      ga_event('Drop Down Content', 'open', page_name);
    } else {
      ga_event('Drop Down Content', 'close', page_name);
    }
  });

  //read more for single page details
  const detailsContent = $('.details-hero-content');
  if (detailsContent) {
    const trimmedContainer = detailsContent.find('.details-content-container');
    let contentHeight = detailsContent.height();
    contentHeight = contentHeight + 100;
    const heroHeight = $('#hero').find('img').height();
    const readmoreBtn = detailsContent.find('.readmore');
    const readlessBtn = detailsContent.find('.readless');

    // console.log(contentHeight ,'>', heroHeight);
    if (contentHeight > heroHeight) {
      detailsContent.addClass('trimmed readmore');
      trimmedContainer.css('max-height', heroHeight - 192 + 'px');
      readmoreBtn.click(function () {
        detailsContent.toggleClass('trimmed');
        trimmedContainer.css('max-height', '');
      });
      readlessBtn.click(function () {
        detailsContent.toggleClass('trimmed');
        trimmedContainer.css('max-height', heroHeight - 192);
      });

    }

  }

  isDocReady = true;
  initPlayer();
});

function sendGAEvent(action, nonInteraction, value) {
  if (value == null) { value = '' };
  if (nonInteraction == null) { nonInteraction = false };

  ga('send', 'event', {
    'eventCategory': 'Youtube Player',
    'eventAction': action,
    'eventLabel': $('#youtube-embed').attr('data-galabel'),
    'eventValue': value,
    'nonInteraction': nonInteraction
  });
}

// Youtube player API
var player;
function onYouTubeIframeAPIReady() {
  isYoutubeReady = true;
  initPlayer();

  // Some browsers (namely FF) do not propagate mouse events over iframes to the parent so we use this div to allow scroll events to occur normally on the homepage
  // However we must catch click events and send the appropriate commands to the youtube player
  $('.youtube-iframe-mouse-event-catch').click(function () {
    if (player.getPlayerState() == YT.PlayerState.PLAYING) {
      player.pauseVideo();
    } else {
      player.playVideo();
    }
  });
}

// Setup the player while preventing YT API and Doc Ready race.
function initPlayer() {
  if (isYoutubePlayerReady) {
    player.loadVideoById(video_container.attr('data-youtube_id'));

    $('.hero-video').fadeIn(600, function () {
      gallery_video_scale();
    });

    return;
  }

  if (isYoutubeReady && isDocReady) {
    var video_container = $('.youtube-hero');

    player = new YT.Player('youtube-iframe', {
      videoId: video_container.attr('data-youtube_id'),
      width: video_container.attr('data-youtube_width'),
      height: video_container.attr('data-youtube_height'),
      playerVars: {
        rel: (video_container.attr('data-youtube_rel') ? video_container.attr('data-youtube_rel') : 0),
        hd: (video_container.attr('data-youtube_hd') ? video_container.attr('data-youtube_hd') : 1),
        autohide: (video_container.attr('data-youtube_autohide') ? video_container.attr('data-autohide') : 1),
        showinfo: (video_container.attr('data-youtube_showinfo') ? video_container.attr('data-youtube_showinfo') : 0),
        autoplay: (video_container.attr('data-youtube_autoplay') ? video_container.attr('data-youtube_autoplay') : 0),
        controls: (video_container.attr('data-youtube_controls') ? video_container.attr('data-youtube_controls') : 1),
        wmode: 'opaque'
      },
      events: {
        'onReady': onPlayerReady,
        'onStateChange': onPlayerStateChange
      }
    });
  }
}

function onPlayerReady(event) {
  sendGAEvent('Player Load', false, '0');
  setInterval(pollYoutubeStats, 100);
}

function onPlayerStateChange(event) {
  if (event.target.getPlayerState() == YT.PlayerState.PLAYING) {
    sendGAEvent('Media Play', false, '0');

    if ($('body').hasClass('home')) {
      $('#scroll-indicator, #home-dot-wrapper').css('opacity', '0');
      $('.youtube-iframe-mouse-event-catch').removeClass('paused');
    }
  }
  if (event.target.getPlayerState() == YT.PlayerState.PAUSED) {
    sendGAEvent('Media Pause', false, '0');

    if ($('body').hasClass('home')) {
      $('.youtube-iframe-mouse-event-catch').addClass('paused');
      $('#scroll-indicator, #home-dot-wrapper').css('opacity', '');
    }
  }
  if (event.target.getPlayerState() == YT.PlayerState.ENDED) {
    sendGAEvent('Percent played', true, 100);

    if ($('body').hasClass('home')) {
      $('#scroll-indicator, #home-dot-wrapper').css('opacity', '');
    }
  }
}

// poll for changes in seeking and volume.
var prevPercentage, prevTime, prevVolume;
prevVolume = undefined;

function pollYoutubeStats() {
  var percent = player.getCurrentTime() / player.getDuration();
  var time = player.getCurrentTime();
  var volume = player.getVolume();

  if (prevVolume == undefined) { // If this is the first loop through, don't track volume change
    prevVolume = player.getVolume();
  }

  if (player.getPlayerState() == YT.PlayerState.PLAYING) {
    if (percent - prevPercentage > 0.01) { // Track every 1% played
      sendGAEvent('Percent played', true, Math.floor(percent));
    }
  }

  if (Math.abs(time - prevTime) > 1) { // If the time has changed more than 1 second, it was probably seeking.
    sendGAEvent(prevTime, 'Seek Start', false, prevTime);
    sendGAEvent('Seek End', false, time);
  }

  if (Math.abs(prevVolume - volume) > 1) { // Track volume changes
    sendGAEvent('Volume Change', false, volume);
  }

  prevTime = time;
  prevPercentage = percent;
  prevVolume = volume;
}

$(window).load(function () {
  flex_row_height();
  taxonomy_shelf_sizing();
  //crumb_2_sizing();
  $('#grid-coverflow-wrapper').css('opacity', '1');
});

function gallery_resize() {
  $('.gallery-inner').each(function () {
    var gallery_width = $(this).width();
    var gallery_height = (gallery_width / 16) * 9;

    $(this).css('height', gallery_height);
  });
}

//calculate taxonomy shelf width
function taxonomy_shelf_sizing() {
  if ($('header .taxonomy-shelf').length) {
    var bread_crumbs_width = $('#breadcrumbs').outerWidth()
    var left_boundary = $('#breadcrumbs').offset().left + bread_crumbs_width;
    var right_boundary = $('.header-top-right').offset().left;
    var tax_shelf_width = right_boundary - left_boundary;
    $('header .taxonomy-shelf').width(tax_shelf_width + 1).css('left', bread_crumbs_width - 1).css('opacity', 1);
  }
}

//calculate breadcrumb 2 width on property detail
function crumb_2_sizing() {
  if ($('body.single').length /*&& !$('body.single-post').length */) {
    var bread_crumbs_first_width = $('.first-crumb').outerWidth();
    var left_boundary = $('.first-crumb').offset().left + bread_crumbs_first_width;
    var right_boundary = $('.header-top-right').offset().left;
    var right_width = $('.header-top-right').outerWidth();
    var bread_crumbs_second_width = right_boundary - left_boundary - right_width;
    $('header .second-crumb').width(bread_crumbs_second_width + 1).css('position', 'absolute').css('left', bread_crumbs_first_width).fadeIn();
    $('#breadcrumbs .social-links').each(function () {
      if (!$(this).hasClass('sharing-container')) {
        $(this).fadeIn();
      }
    });
  }
}

//updates grid image loading
function updates_grid_images(size) {
  $('.update-grid-item').each(function () {
    var grid_item = $(this);
    var grid_image = grid_item.data('image');
    if (size !== 'desktop') {
      var grid_image = grid_item.data('responsive_image');
    }
    $('.resize-overlay', grid_item).show();
    var img = new Image();
    img.onload = function () {
      grid_item.css("background-image", "url(" + grid_image + ")");
      $('.resize-overlay', grid_item).fadeOut();
    };
    img.src = grid_image;
  });
}

// image swapping for responsiveness
function resp_img_swap(size) {
  //hero images
  $('.hero-image').each(function () {
    var current_img = $('img', this).attr('src');
    var desktop = $(this).data('desktop_image');
    var tablet = $(this).data('tablet_image');
    var mobile = $(this).data('mobile_image');

    $('.resize-overlay', this).show();

    switch (size) {

      case "mobile":
        if (mobile !== current_img) {

          $('img', this).one("load", function () {
            $(this).parent('.hero-image').children('.resize-overlay').fadeOut();
          }).attr("src", mobile);

        }
        else {
          $('.resize-overlay', this).fadeOut();
        }
        break;

      case "tablet":
        if (tablet !== current_img) {

          $('img', this).one("load", function () {
            $(this).parent('.hero-image').children('.resize-overlay').fadeOut();
          }).attr("src", tablet);

        }
        else {
          $('.resize-overlay', this).fadeOut();
        }
        break;

      case "desktop":

        if (desktop !== current_img) {

          $('img', this).one("load", function () {
            $(this).parent('.hero-image').children('.resize-overlay').fadeOut();
          }).attr("src", desktop);

        }
        else {
          $('.resize-overlay', this).fadeOut();
        }
        break;
    }

  });
}

// image swapping for homepage responsiveness
function hero_img_swap(size) {

  //hero images
  $('.hero-item').each(function () {
    var current_img = $(this).css('background-image');
    current_img = current_img.replace('url(', '').replace(')', '');
    var desktop = $(this).data('desktop_image');
    var tablet = $(this).data('tablet_image');
    var mobile = $(this).data('mobile_image');

    var withVideo = $(this).parent().hasClass('with-video');
    if (withVideo) {
      // $('.resize-overlay', $(this)).fadeOut();
      return;
    }

    var homepage_item = $(this);
    $('.resize-overlay', homepage_item).show();
    switch (size) {

      case "mobile":
        if (mobile !== current_img) {

          var img = new Image();
          img.onload = function () {
            homepage_item.css("background-image", "url(" + mobile + ")");
            $('.resize-overlay', homepage_item).fadeOut();
          };
          img.src = mobile;

        }
        else {
          $('.resize-overlay', homepage_item).fadeOut();
        }
        break;

      case "tablet":
        if (tablet !== current_img) {

          var img = new Image();
          img.onload = function () {
            homepage_item.css("background-image", "url(" + tablet + ")");
            $('.resize-overlay', homepage_item).fadeOut();
          };
          img.src = tablet;

        }
        else {
          $('.resize-overlay', homepage_item).fadeOut();
        }
        break;

      case "desktop":

        if (desktop !== current_img) {

          var img = new Image();
          img.onload = function () {
            homepage_item.css("background-image", "url(" + desktop + ")");
            $('.resize-overlay', homepage_item).fadeOut();
          };
          img.src = desktop;

        }
        else {
          $('.resize-overlay', homepage_item).fadeOut();
        }
        break;
    }

  });
}

//homepage item tablet landscape height
function home_item_height() {
  if ($(window).width() == '1024' && $('.hero-item').length) {
    var window_height = $(window).height();
    $('.hero-item').each(function () {
      $(this).height(window_height);
    });
  } else if ($('.hero-item').length) {
    $('.hero-item').each(function () {
      $(this).height('');
    });
  }
}

//video scaling
function video_scale() {
  if ($('.flex-video iframe').length) {
    $('.flex-video').each(function () {
      var width = $('iframe', this).width();
      var height = (width / 16) * 9;
      $('iframe', this).height(height - 100);
    });
  } else if ($('.flex-video .video-js').length) {
    $('.flex-video').each(function () {
      var width = $('.video-js', this).width();
      var height = (width / 16) * 9;
      $('.video-js', this).height(height);
    });
  }
}

//hero video scaling
function hero_video_scale() {
  if ($('.hero-video iframe').length) {
    $('.hero-video').each(function () {
      var width = $('iframe', this).width();
      var height = (width / 16) * 9;
      $('iframe', this).height(height);
    });
  }
  else if ($('#hero-brightcove-container .video-js').length) {
    $('#hero-brightcove-container').each(function () {
      var width = $('.video-js', this).width();
      var height = (width / 16) * 9;
      $('.video-js', this).height(height);
    });
  }
}

function hero_wrapper_scale(width_ratio, height_ratio) {
  // console.log('hero wrapper scale running')
  //   if($('.page-template-page_template_content_landing #hero_wrapper').length){
  //       setTimeout(() => {
  //         var width = $('#hero-wrapper').width();
  //         console.log('el: ', $('#hero-wrapper'));
  //         console.log('width: ', width);
  //         console.log('width_ratio: ', width_ratio);
  //         console.log('height_ratio: ', height_ratio);

  //         var height =  (width / width_ratio) * height_ratio;
  //         console.log('setting hero height', height);
  //         $('#hero_wrapper').height(height);
  //       }, 300);
  //   }
}

//taxonomy shelf scrolling effect
var taxonomy_shelf_waypoint = $('.page-template-page_template_content_landing #hero_wrapper').waypoint({
  handler: function (direction) {
    if (direction == "down") {
      $('header .taxonomy-shelf').addClass('open');
    } else {
      $('header .taxonomy-shelf').removeClass('open');
      $('header .taxonomy-shelf .shelf-dropdown.open').removeClass('open');
    }
  },
  offset: '-30%'
});

//grid filters
$('#grid-filters a').click(function () {
  $('#grid-filters a').removeClass('active');
  $(this).addClass('active');
  var filter_name = $(this).data('filter_type');
  if (filter_name == 'all') {
    $('.property-item').show().removeClass('hidden');
  }
  else {
    $('#properties-wrapper.grid .poster-wrapper').css('opacity', '1');
    $('.property-item').each(function () {
      var filters = $(this).data('filter_3');
      if (filters.indexOf(filter_name) == -1) {
        $(this).hide().addClass('hidden');
      }
      else {
        $(this).show().removeClass('hidden');
      }
    });
  }
});

//coverflow function
function initialize_coverflow() {
  if (disable_coverflow !== true) {
    var item_num = $('.property-item').length;
    var item_mid = Math.round((item_num / 2) - 1);
    $('#properties-wrapper').removeAttr('class').addClass('cover-flow');
    $('#properties-list').coverflow();

    //focus on middle item
    //$('#properties-list').coverflow('index',item_mid);

    //focus on first item
    $('#properties-list').coverflow();

    var cover_flow_height = $(window).height();
    var cover_height = $('.property-item').height();
    $('#properties-wrapper').height(cover_height + 150);
    $('#properties-list').height(cover_height);
    $('#properties-toggle a').removeClass('active');
    $('#cover-flow-toggle').addClass('active');
  }
}
function refresh_coverflow() {
  if (disable_coverflow !== true) {
    $('#properties-list').coverflow('refresh');
    $('#properties-list').coverflow('index', 0);
  }
}
function destroy_coverflow() {
  if (disable_coverflow !== true) {
    $('#properties-list').coverflow('destroy');
    $('#properties-list').css('height', 'auto');
    $('.property-item').attr('style', '').css('display', 'inline').removeClass('hidden');
    $('#properties-wrapper').css('height', 'auto');
  }
}
function base_toggle(this_thing, display_type) {
  $('#properties-toggle a').removeClass('active');
  $(this_thing).addClass('active');
  $('#properties-wrapper').removeAttr('class').addClass(display_type);

  //ga event
  var page_name = $('.first-crumb').text();
  ga_event('Posters', display_type + ' toggle', page_name);
}

function clear_all_filters() {
  $('.property-item').show().attr('style', '').css('display', 'inline').removeClass('hidden');
  $('.shelf-button').text('Browse by Category');
  $('.shelf-dropdown a').removeClass('active');
  $('.cover-flow-filters a, #grid-filters a').removeClass('active');
  $('.reset-filters').addClass('active');
}

//set flex row heights
function flex_row_height() {
  if ($('.flexible-content-row').length) {
    $('.flexible-content-row').attr('style', '');
    $('.cta-img-wrapper img').attr('style', '');
    if ($(window).width() > 1023) {
      $('.flexible-content-row').each(function () {
        // var heights_array = new Array();
        // $('.flexible-content-item', this).each(function(){
        //     var item_height = $(this).height();
        //     heights_array.push(item_height);
        // });
        // var row_height = Math.max.apply(Math, heights_array);
        // $(this).css('height', row_height);

        // //set img heights for cta module

        // if($('.cta-img-wrapper img', this).length){
        //     $('.cta-img-wrapper img', this).css('height', row_height);
        // }
      });
    }
  }
}

//flex content scrolling effect
$(window).load(function () {
  var flex_content_waypoint = $('.flexible-content-row').waypoint({
    handler: function (direction) {
      if (direction == "down") {
        $('.flexible-content-item', this.element).repeat().each($).css('opacity', '1').wait(500);
      }
    },
    offset: '85%'
  });
});

//poster image (vertical landing page) scrolling effect
$(window).load(function () {
  var poster_image_waypoint = $('#properties-wrapper.grid .poster-wrapper').waypoint({
    handler: function (direction) {
      if (direction == "down") {
        $(this.element).css('opacity', '1');
      }
    },
    offset: '85%'
  });
});

//landing hero scrolling effect
// var landing_hero_waypoint = $('.page-template-page_template_content_landing #hero_wrapper #hero-wrapper-trigger').waypoint({
//     handler: function(direction) {
//         if(direction=="down"){
//             $('#hero_wrapper').css('opacity','0');
//         } else {
//             $('#hero_wrapper').css('opacity','1');
//         }
//     },
//     offset: 0
// });

//lazy loading. :(
var lazy_loading_waypoint = $('#more-updates-wrapper #more-updates-trigger').waypoint({
  handler: function (direction) {
    if (direction == "down" && $('#load-more-posts-button').is(":visible")) {
      $('#load-more-posts-button').trigger('click');
    }
  },
  offset: 'bottom-in-view'
});

//pull twitter feeds
$(document).ready(function () {

  if (document.location.hostname == 'localhost' || document.location.hostname !== 'localhost') {
    //pull uncached twitter (local dev) *** NEED TO TURN OVER TO CACHED WHEN SERVER AUTHENTICATION IS TURNED OFF ***

    var tw_handle = 'legendary';
    var tw_count = 1;
    var url_path = '/wp-content/themes/legendarydotcom/inc/twitter.php'
    if (document.location.hostname == 'localhost') {
      var url_path = '/legendary/legendarydotcom/wp-content/themes/legendarydotcom/inc/twitter.php'
    }
    $('.twitter-feed').each(function () {
      var this_feed = $(this);
      tw_handle = this_feed.data('feed_handle');
      $.ajax({
        url: url_path,
        type: 'GET',
        dataType: 'json',
        data: {
          method: "user",
          search: tw_handle,
          count: tw_count,
          include_rts: true
        },
        success: function (data) {
          for (var i = 0; i < data.length; i++) {
            tweetText = data[i].text;
            $(this_feed).append('<span class="vertical">' + tweetText + '</span>');
            flex_row_height();
          }
        }
      });
    });
  } else {

    //pull cached twitter feed (wpe servers)
    var tw_handle = 'legendary';
    var tw_count = 1;

    $('.twitter-feed').each(function () {
      var this_feed = $(this);
      tw_handle = this_feed.data('feed_handle');
      var url_path = '/wp-content/themes/legendarydotcom/inc/twitter_cache.php?twitter_account=' + tw_handle;
      $.ajax({
        url: url_path,
        type: 'GET',
        dataType: 'json',
        data: {},
        success: function (data) {
          for (var i = 0; i < data.length; i++) {
            tweetText = data[i].text;
            $(this_feed).append('<span class="vertical">' + tweetText + '</span>');
            flex_row_height();
          }
        }
      });
    });
  }
});

// instagram feeds

$(document).ready(function () {
  $('.instagram-feed').each(function () {
    var this_feed = $(this);
    var account_id = this_feed.data('feed_handle');
    $(this_feed).next('h2').hide();
    $(this_feed).parents('.instagram-flex-content').children('.cta-img-wrapper').children('img').hide();
    $(this_feed).parents('.instagram-flex-content').children('a.flex-social-link').hide();
    var json_type = "json";
    var feed_url = '/wp-content/themes/legendarydotcom/inc/instagram_cache.php?account_id=' + account_id;
    if (document.location.hostname == 'localhost') {
      // if local dev - pull uncached feed
      json_type = "jsonp";
      feed_url = "https://api.instagram.com/v1/users/" + account_id + "/media/recent/?access_token=1604978666.3985b44.adedf75c4b454b669f580f69ec129504";
    }
    $.ajax({
      type: "GET",
      dataType: json_type,
      cache: false,
      url: feed_url,
      success: function (data) {
        for (var i = 0; i < 1; i++) {
          $(this_feed).next('h2').text('@' + data.data[i].user.username).fadeIn();
          $(this_feed).parents('.instagram-flex-content').children('a.flex-social-link').attr('href', data.data[i].link).show();
          $(this_feed).parents('.instagram-flex-content').children('.cta-img-wrapper').children('img').attr('src', data.data[i].images.standard_resolution.url).fadeIn();
          if (data.data[i].caption.text) {
            $(this_feed).append(data.data[i].caption.text);
          }
          flex_row_height();
        }
      }
    });
  });
});


//pull tumblr feeds
$(document).ready(function () {
  $('.tumblr-feed').each(function () {
    var this_feed = $(this);
    var account_id = this_feed.data('feed_handle');
    var json_type = "json";
    var feed_url = '/wp-content/themes/legendarydotcom/inc/tumblr_cache.php?account_id=' + account_id;
    if (document.location.hostname == 'localhost') {
      // if local dev - pull uncached feed
      json_type = "jsonp";
      feed_url = "https://api.tumblr.com/v2/blog/" + account_id + "/posts/?api_key=zLbGKjBJEyCQQ9KhDfTEI78ur4wJ9VqNItaJqFM0nrC8viNyEv";
    }
    $.ajax({
      type: "GET",
      dataType: json_type,
      cache: false,
      url: feed_url,
      success: function (data) {
        for (var i = 0; i < 1; i++) {
          var post_object = data.response.posts[i];
          //console.log(post_object);
          $(this_feed).parents('.tumblr-flex-content').children('a.flex-social-link').attr('href', post_object.post_url);
          if (post_object.photos && post_object.photos[0]) {
            $(this_feed).parents('.tumblr-flex-content').css('background-image', 'url(' + post_object.photos[0].original_size.url + ')');
          }
          if (post_object.title) {
            $(this_feed).append(strip_html(post_object.title));
          } else if (post_object.caption) {
            $(this_feed).append(strip_html(post_object.caption));
          }
          flex_row_height();
        }
      }
    });
  });
});

function strip_html(string_to_edit) {
  var regex = /(<([^>]+)>)/ig;
  return string_to_edit.replace(regex, "");
}

//homepage dot positioning
function home_dots_position() {
  var dots_container_height = $('#home-dots').height();
  var dots_container_height_half = dots_container_height / 2;
  var dots_container_height_half_neg = 0 - dots_container_height_half;
  $('#home-dots').css('margin-top', dots_container_height_half_neg);
}

//flexslider hover focus
$('.page-template-page_template_content_landing #hero_wrapper li a, .page-template-page_template_ldn_landing #hero_wrapper li a').hover(function () {
  $(this).focusWithoutScrolling();
}, function () {
  $(this).blur();
});

$.fn.focusWithoutScrolling = function () {
  var x = window.scrollX, y = window.scrollY;
  this.focus();
  window.scrollTo(x, y);
  return this; //chainability

};

//smartquest title cropping
function smartquest_title_crop() {
  /*
      if($('#smartquest').length){
          $('#smartquest .update-grid-item h2').each(function(){
              var element = $(this);
              //element.wrapInner('<span class="smartquest-title"></span>');
              var element_height = $(this).outerHeight();
              var content_height = $('span', this).outerHeight();
              var element_width = $(this).outerWidth();
              var content_width = $('span', this).outerWidth();
              if (element_height < content_height ||
              element_width < content_width) {
                  $(this).addClass('overflow');
              } else {
                  $(this).removeClass('overflow');
              }
          });
      }
  */
}

//url query strings - pop open modules based on url parameters
$(window).load(function () {

  if (QueryString.t) {
    var mod_trigger = QueryString.t;
    if (mod_trigger == 'hero-video') {
      $('#hero-video-wrapper').trigger('click');
    } else {
      $("html, body").animate({
        scrollTop: $('*[data-unique_string="' + mod_trigger + '"]').offset().top
      }, 600, function () {
        $('*[data-unique_string="' + mod_trigger + '"]').trigger('click');
      });
    }
  }
});

//share button toggle
$('a.icon-share').click(function () {
  $(this).nextAll('.sharing-container').slideToggle();
});

var QueryString = function () {
  // This function is anonymous, is executed immediately and
  // the return value is assigned to QueryString!
  var query_string = {};
  var query = window.location.search.substring(1);
  var vars = query.split("&");
  for (var i = 0; i < vars.length; i++) {
    var pair = vars[i].split("=");
    // If first entry with this name
    if (typeof query_string[pair[0]] === "undefined") {
      query_string[pair[0]] = pair[1];
      // If second entry with this name
    } else if (typeof query_string[pair[0]] === "string") {
      var arr = [query_string[pair[0]], pair[1]];
      query_string[pair[0]] = arr;
      // If third or later entry with this name
    } else {
      query_string[pair[0]].push(pair[1]);
    }
  }
  return query_string;
}();

//Custom Google Tracking
//some is mixed throughout above

//logo click
$('#logo').click(function () {
  ga_event('Logo', 'click', '');
});

//main nav item clicks
$('#main-menu a').click(function () {
  var event_label = $(this).attr('href');
  ga_event('Main Nav', 'click', event_label);
});

//homepage item clicks
$('.home a.hero-item').click(function () {
  var event_label = $(this).attr('href');
  ga_event('Homepage Item', 'click', event_label);
});

//Vertical landing page carousel click
$('.page-template-page_template_content_landing a.hero-item, .page-template-page_template_ldn_landing a.hero-item').click(function () {
  var page_name = $('.first-crumb').text();
  var slide_link = $(this).attr('href');
  var event_label = 'Page: ' + page_name + ' | Link: ' + slide_link;
  ga_event('Carousel', 'click', event_label);
});

//vertical landing page poster click
$('.poster-link').click(function () {
  var page_name = $('.first-crumb').text();
  var slide_link = $(this).attr('href');
  var event_label = 'Page: ' + page_name + ' | Link: ' + slide_link;
  if ($('#properties-wrapper').hasClass('cover-flow')) {
    var display_type = 'cover-flow';
  } else {
    var display_type = 'grid';
  }
  ga_event('Posters', display_type + ' click', event_label);
});

//search results clicks

$('.search a.update-grid-item').click(function () {
  var destination = $(this).attr('href');
  var search_term = $('#s-results').data('search_term');
  var event_label = 'Search Term: ' + search_term + ' | Link: ' + destination;
  ga_event('Search Result', 'click', event_label);
});

//latest news items clicks
$('.page-template-page_template_updates').on('click', '.update-grid-item', function (e) {
  var destination = $(this).attr('href');
  ga_event('Latest News', 'item click', destination);
});

//ldn item clicks
$('.ldn-item .ldn-image-wrapper a, .ldn-item .ldn-content a.button').click(function () {
  if ($(this).hasClass('button')) {
    var ga_action = 'buton click';
  } else {
    var ga_action = 'image click';
  }
  var destination = $(this).attr('href');
  ga_event('LDN Item', ga_action, destination);
});

//detail page social channels links
$('.second-crumb .social-links > ul > li > a, .in-page-social .social-links > ul > li > a').click(function () {
  var page_name = $('h1#item-detail-title').text();
  var destination = $(this).attr('href');
  var event_label = 'Page: ' + page_name + ' | Link: ' + destination;
  if (destination !== 'javascript:void(0)') {
    ga_event('Social Channel Link', 'click', event_label);
  }
});

//detail page social sharing
/*
$('.sharing-container.main_page > ul > li > a').click(function(){
    var page_name = $('h1#item-detail-title').text();
    var ga_action = $(this).text();
    ga_event('Page Share', ga_action, page_name);
});
*/

//social module clicks
$('.flex-social-link').click(function () {
  var page_name = $('h1#item-detail-title').text();
  var destination = $(this).attr('href');
  var event_label = 'Page: ' + page_name + ' | Link: ' + destination;
  ga_event('Social Module', 'click', event_label);
});

//smartquest clicks
$('#smartquest .update-grid-item').click(function () {
  var page_name = $('h1#item-detail-title').text();
  var destination = $(this).attr('href');
  var event_label = 'Page: ' + page_name + ' | Link: ' + destination;
  ga_event('Smartquest', 'click', event_label);
});

//cta module clicks
$('.cta-module-link').click(function () {
  var page_name = $('h1#item-detail-title').text();
  var destination = $(this).attr('href');
  var event_label = 'Page: ' + page_name + ' | Link: ' + destination;
  ga_event('CTA Module', 'click', event_label);
});

function ga_pageview(tracking_path) {
  //    _gaq.push(['_trackPageview', window.location.pathname+tracking_path]);
}

function ga_event(event_category, event_action, event_label) {
  if (typeof (ga) == "function") {
    ga('send', 'event', event_category, event_action, event_label);
  }
}

function detectswipe(el, func) {
  swipe_det = new Object();
  swipe_det.sX = 0; swipe_det.sY = 0; swipe_det.eX = 0; swipe_det.eY = 0;
  var min_x = 30;  //min x swipe for horizontal swipe
  var max_x = 30;  //max x difference for vertical swipe
  var min_y = 50;  //min y swipe for vertical swipe
  var max_y = 60;  //max y difference for horizontal swipe
  var direc = "";
  // ele = document.querySelectorAll(el);

  ele = el;
  ele.addEventListener('touchstart', function (e) {
    var t = e.touches[0];
    swipe_det.sX = t.screenX;
    swipe_det.sY = t.screenY;
  }, false);
  ele.addEventListener('touchmove', function (e) {
    // e.preventDefault();
    var t = e.touches[0];
    swipe_det.eX = t.screenX;
    swipe_det.eY = t.screenY;
  }, false);
  ele.addEventListener('touchend', function (e) {
    //horizontal detection
    if ((((swipe_det.eX - min_x > swipe_det.sX) || (swipe_det.eX + min_x < swipe_det.sX)) && ((swipe_det.eY < swipe_det.sY + max_y) && (swipe_det.sY > swipe_det.eY - max_y) && (swipe_det.eX > 0)))) {
      if (swipe_det.eX > swipe_det.sX) direc = "r";
      else direc = "l";
    }
    //vertical detection
    else if ((((swipe_det.eY - min_y > swipe_det.sY) || (swipe_det.eY + min_y < swipe_det.sY)) && ((swipe_det.eX < swipe_det.sX + max_x) && (swipe_det.sX > swipe_det.eX - max_x) && (swipe_det.eY > 0)))) {
      if (swipe_det.eY > swipe_det.sY) direc = "d";
      else direc = "u";
    }

    else {
      direc = "none";
    }


    if (direc != "") {
      if (typeof func == 'function') func(el, direc);
    }
    direc = "";
    swipe_det.sX = 0; swipe_det.sY = 0; swipe_det.eX = 0; swipe_det.eY = 0;
  }, false);
}

function getScrollbarWidth() {
  var scrollDiv = document.createElement("div");
  scrollDiv.className = "scrollbar-measure";
  document.body.appendChild(scrollDiv);

  // Get the scrollbar width
  var scrollbarWidth = scrollDiv.offsetWidth - scrollDiv.clientWidth;
  // Delete the DIV
  document.body.removeChild(scrollDiv);

  return scrollbarWidth;

}

