$(document).ready(function(){
  const body = document.querySelector('body');
  const windowWidth = body.getBoundingClientRect().width;
  const landingNewsCarousel = document.querySelector('.contentLanding-newsCarousel');
  const $whereToTrigger = $('.contentMeta-actionsWhereTrigger');
  const $whereToDropdown = $('.contentMeta-actionsWhere-dropdown');
  const newsListing = $('ul.feed-catalog--slim');
  const contentButtons = $('.flexible-box-content .button');

  contentButtons.each((i, el) => {
    const contents = $(el).contents();
    let hasIcon = false;

    if (contents.length > 1) {
      contents.each((i, nodeItem) => {
        if (nodeItem.nodeName === 'IMG') {
          hasIcon = true;
        }
      })
    }

    if (hasIcon) {
      $(el).addClass('has-icons');
    }
  });

  $(".videoCard-item").colorbox({
    rel: 'videoGallery',
    iframe: true,
    width: '70%',
    height: '70%'
  });

  $(".photoCard-btn").colorbox({
    rel:'gallery',
    maxHeight: '90%',
    maxWidth: '90%'
  });
  $(window).click(function() {
    if ($whereToDropdown.hasClass('isOpen')) {
      $whereToDropdown.removeClass('isOpen');
    }
  });
  $whereToTrigger.on('click', (e) => {
    e.stopPropagation();
    $whereToDropdown.toggleClass('isOpen');
  });

  if (document.querySelector('.section.singleDetailVideos')) {
    const section = document.querySelector('.section.singleDetailVideos');
    const numShown = section.getAttribute('data-num');
    const contentWidth = section.querySelector('.sectionContent').getBoundingClientRect().width;
    const controls = Array.from(section.querySelectorAll('.sectionContent-controls > button.sectionContent-control'));
    const children = Array.from(section.querySelectorAll('.sectionContent-items .sectionItem'));
    let isMobile = windowWidth <= 768;
    let flexsliderInstance = null;

    if (children.length > numShown) {
      section.classList.remove('section-hideControls');
    }

    const flexslider = $('.sectionContent-items--slider', section).flexslider({
      animation: 'slide',
      animationLoop: false,
      minItems: isMobile ? 1 : numShown,
      maxItems: isMobile ? 1 : numShown,
      itemWidth: isMobile ? contentWidth : (contentWidth / numShown),
      itemMargin: 0,
      slideshow: false,
      controlNav: true,
      customDirectionNav: false,
      smoothHeight: isMobile ? true : false,
      start: function(slider){
        flexsliderInstance = slider;
        if (isMobile) {
          slider.vars.minItems = 1;
          slider.vars.maxItems = 1;
          slider.vars.itemWidth = contentWidth;
        }
        slider.resize();
      }
    });

    controls.forEach(control => {
      control.addEventListener('click', (e) => {
        e.preventDefault();
        const isPrev = e.target.classList.contains('sectionContent-controlPrev');
        flexslider.flexslider(isPrev ? 'prev' : 'next');
      });
    });

    window.addEventListener('resize', () => {
      const windowWidth = document.querySelector('body').getBoundingClientRect().width;

      // If was desktop and changed to mobile
      if (windowWidth <= 768 && !isMobile) {
        isMobile = true;
        flexsliderInstance.vars.minItems = 1;
        flexsliderInstance.vars.maxItems = 1;
        flexsliderInstance.vars.itemWidth = contentWidth;
        flexsliderInstance.vars.smoothHeight = true;
        flexsliderInstance.resize();
      }

      // If was mobile and changed to desktop
      if (windowWidth > 768 && isMobile) {
        isMobile = false;
        flexsliderInstance.vars.minItems = numShown;
        flexsliderInstance.vars.maxItems = numShown;
        flexsliderInstance.vars.itemWidth = (contentWidth / numShown);
        flexsliderInstance.resize();
      }
    }, false);
  }

  if (document.querySelector('.section.singleDetailRelatedNews')) {
    const section = document.querySelector('.section.singleDetailRelatedNews');
    const numShown = section.getAttribute('data-num');
    const contentWidth = section.querySelector('.sectionContent').getBoundingClientRect().width;
    const controls = Array.from(section.querySelectorAll('.sectionContent-controls > button.sectionContent-control'));
    const children = Array.from(section.querySelectorAll('.sectionContent-items .sectionItem'));
    let isMobile = windowWidth <= 768;
    let flexsliderInstance = null;

    if (children.length > numShown) {
      section.classList.remove('section-hideControls');
    }

    const flexslider = $('.sectionContent-items--slider', section).flexslider({
      animation: 'slide',
      animationLoop: false,
      minItems: isMobile ? 1 : numShown,
      maxItems: isMobile ? 1 : numShown,
      itemWidth: isMobile ? contentWidth : (contentWidth / numShown),
      itemMargin: 0,
      slideshow: false,
      controlNav: true,
      customDirectionNav: false,
      smoothHeight: isMobile ? true : false,
      start: function(slider){
        flexsliderInstance = slider;
        if (isMobile) {
          slider.vars.minItems = 1;
          slider.vars.maxItems = 1;
          slider.vars.itemWidth = contentWidth;
        }
        slider.resize();
      }
    });

    controls.forEach(control => {
      control.addEventListener('click', (e) => {
        e.preventDefault();
        const isPrev = e.target.classList.contains('sectionContent-controlPrev');
        flexslider.flexslider(isPrev ? 'prev' : 'next');
      });
    });

    window.addEventListener('resize', () => {
      const windowWidth = document.querySelector('body').getBoundingClientRect().width;

      // If was desktop and changed to mobile
      if (windowWidth <= 768 && !isMobile) {
        isMobile = true;
        flexsliderInstance.vars.minItems = 1;
        flexsliderInstance.vars.maxItems = 1;
        flexsliderInstance.vars.itemWidth = contentWidth;
        flexsliderInstance.vars.smoothHeight = true;
        flexsliderInstance.resize();
      }

      // If was mobile and changed to desktop
      if (windowWidth > 768 && isMobile) {
        isMobile = false;
        flexsliderInstance.vars.minItems = numShown;
        flexsliderInstance.vars.maxItems = numShown;
        flexsliderInstance.vars.itemWidth = (contentWidth / numShown);
        flexsliderInstance.resize();
      }
    }, false);
  }

  // Detail Landing Pages
  if (landingNewsCarousel) {
    const numShown = landingNewsCarousel.getAttribute('data-num');
    const contentWidth = landingNewsCarousel.querySelector('.sectionContent').getBoundingClientRect().width;
    const controls = Array.from(landingNewsCarousel.querySelectorAll('.sectionContent-controls > button.sectionContent-control'));
    const children = Array.from(landingNewsCarousel.querySelectorAll('.sectionContent-items .sectionItem'));
    let isMobile = windowWidth <= 768;
    let flexsliderInstance = null;

    if (children.length > numShown) {
      landingNewsCarousel.classList.remove('section-hideControls');
    }

    const flexslider = $('.sectionContent-items--slider', landingNewsCarousel).flexslider({
      animation: 'slide',
      animationLoop: false,
      minItems: isMobile ? 1 : numShown,
      maxItems: isMobile ? 1 : numShown,
      itemWidth: isMobile ? contentWidth : (contentWidth / numShown),
      itemMargin: 0,
      slideshow: false,
      controlNav: true,
      customDirectionNav: false,
      start: function(slider){
        flexsliderInstance = slider;
        if (isMobile) {
          slider.vars.minItems = 1;
          slider.vars.maxItems = 1;
          slider.vars.itemWidth = contentWidth;
        }
        slider.resize();
      }
    });

    controls.forEach(control => {
      control.addEventListener('click', (e) => {
        e.preventDefault();
        const isPrev = e.target.classList.contains('sectionContent-controlPrev');
        flexslider.flexslider(isPrev ? 'prev' : 'next');
      });
    });

    window.addEventListener('resize', () => {
      const windowWidth = document.querySelector('body').getBoundingClientRect().width;

      // If was desktop and changed to mobile
      if (windowWidth <= 768 && !isMobile) {
        isMobile = true;
        flexsliderInstance.vars.minItems = 1;
        flexsliderInstance.vars.maxItems = 1;
        flexsliderInstance.vars.itemWidth = contentWidth;
        flexsliderInstance.resize();
      }

      // If was mobile and changed to desktop
      if (windowWidth > 768 && isMobile) {
        isMobile = false;
        flexsliderInstance.vars.minItems = numShown;
        flexsliderInstance.vars.maxItems = numShown;
        flexsliderInstance.vars.itemWidth = (contentWidth / numShown);
        flexsliderInstance.resize();
      }
    }, false);
  }

  // News Landing
  if (newsListing) {
    const newsItems = newsListing.find('li.sectionItem');
    const newsItemsLength = newsItems.length;
    const showMoreBtn = $('.feed-catalog-showMoreBtn');
    let visibleCount = 15;

    // Initial
    newsItems.each((i, el) => {
      if (i > 9) {
        $(el).addClass('isHidden');
      }
    });

    showMoreBtn.on('click', () => {
      const hiddenItems = newsListing.find('li.sectionItem.isHidden');

      hiddenItems.each((i, el) => {
        if (i > 9) {
          return;
        }
        $(el).removeClass('isHidden').css('opacity', 0);
        setTimeout(() => {
          $(el).css('opacity', 1);
        }, 100 * i);
        visibleCount++;
      });

      if (visibleCount >= newsItemsLength) {
        showMoreBtn.css('opacity', 0).addClass('isHidden');
      }
    });
  }
});
