$(document).ready(function(){
  const body = document.querySelector('body');
  const windowWidth = document.querySelector('body').getBoundingClientRect().width;

  // Check that we're on the homepage to run this
  if (body.classList.contains('home')) {
    const tl = new TimelineMax();
    const spotlight = document.querySelector('.section-spotlight');
    const sections = Array.from(document.querySelectorAll('.section-carousel'));

    // Spotlight functionality
    if (spotlight) {
      const grids = Array.from(spotlight.querySelectorAll('.spotlight-grid'));
      const spotlightGridItems = Array.from(spotlight.querySelectorAll('.grid-item'));
      let spotlightAnimating = false;

      if (grids.length > 1) {
        const controls = spotlight.querySelectorAll('.sectionContent-controls .sectionContent-control');
        const pages = spotlight.querySelectorAll('.sectionContent-pager li button');

        spotlight.classList.remove('section-hideControls');
        controls.forEach(control => {
          control.addEventListener('click', e => {
            const target = e.target;
            const isPrev = target.classList.contains('sectionContent-controlPrev');
            const activeGrid = grids.filter(grid => {
              return grid.classList.contains('active');
            })[0];
            const activeGridIndex = grids.indexOf(activeGrid);
            let gridItems = [];

            if (!activeGrid || spotlightAnimating) {
              return;
            }
            spotlightAnimating = true;
            gridItems = Array.from(activeGrid.querySelectorAll('.grid-item'));

            tl.staggerTo(gridItems, 0.25, {
              opacity: 0,
              left: isPrev ? -10 : 10
            }, 0.1, '+=0', () => {
              let newActiveGrid;
              let newActiveGridIndex;
              let newGridItems = [];

              if (isPrev) {
                if (activeGridIndex === 0) {
                  newActiveGridIndex = grids.length - 1;
                } else {
                  newActiveGridIndex = activeGridIndex - 1;
                }
              } else {
                if (activeGridIndex === grids.length - 1) {
                  newActiveGridIndex = 0;
                } else {
                  newActiveGridIndex = activeGridIndex + 1;
                }
              }
              newActiveGrid = grids[newActiveGridIndex];

              pages.forEach((page, i) => {
                page.classList.remove('active');
                if (i === newActiveGridIndex) {
                  page.classList.add('active');
                }
              });
              newGridItems = Array.from(newActiveGrid.querySelectorAll('.grid-item'));

              tl.staggerFromTo(newGridItems, 0.25, {
                opacity: 0,
                left: isPrev ? 10 : -10
              }, {
                opacity: 1,
                left: 0
              }, 0.1, '+=0', () => {
                spotlightAnimating = false;
              });
              activeGrid.classList.remove('active');
              newActiveGrid.classList.add('active');
            });
          }, false);
        });
        pages.forEach(page => {
          page.addEventListener('click', e => {
            const target = e.target;
            const targetIndex = target.getAttribute('data-index') - 1;
            const activeGrid = grids.filter(grid => {
              return grid.classList.contains('active');
            })[0];
            const activeGridIndex = grids.indexOf(activeGrid);
            const isPrev = targetIndex < activeGridIndex;

            if (activeGridIndex === targetIndex || !activeGrid || spotlightAnimating) {
              return;
            }
            spotlightAnimating = true;
            gridItems = Array.from(activeGrid.querySelectorAll('.grid-item'));

            pages.forEach(page => {
              page.classList.remove('active');
            });
            target.classList.add('active');

            tl.staggerTo(gridItems, 0.25, {
              opacity: 0,
              left: isPrev ? -10 : 10
            }, 0.1, '+=0', () => {
              const newActiveGrid = grids[targetIndex];
              let newGridItems = Array.from(newActiveGrid.querySelectorAll('.grid-item'));

              tl.staggerFromTo(newGridItems, 0.25, {
                opacity: 0,
                left: isPrev ? 10 : -10
              }, {
                opacity: 1,
                left: 0
              }, 0.1, '+=0', () => {
                spotlightAnimating = false;
              });
              activeGrid.classList.remove('active');
              newActiveGrid.classList.add('active');
            });
          }, false);
        });
      }
      spotlightGridItems.forEach(card => {
        let isAnimating = false;

        if (!card.classList.contains('grid--videoCard')) {
          card.addEventListener('mouseenter', (e) => {
            const img = card.querySelector('.cardBG');
            const rect = card.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;

            if (windowWidth <= 768) {
              return;
            }
            isAnimating = true;

            gsap.fromTo(img, {
              x: 0,
              y: 0
            }, {
              x: `-${x/50}px`,
              y: `-${y/35}px`,
              duration: 0.3,
              onComplete: () => {
                isAnimating = false;
              }
            });
          }, false);
          card.addEventListener('mousemove', (e) => {
            const img = card.querySelector('.cardBG');
            const rect = card.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;

            if (isAnimating || windowWidth <= 768) {
              return;
            }
            gsap.fromTo(img, {
              x: img._gsap.x,
              y: img._gsap.y,
            },{
              x: `-${x/50}px`,
              y: `-${y/35}px`,
              duration: 0.15
            });
          }, false);
          card.addEventListener('mouseleave', () => {
            const img = card.querySelector('.cardBG');

            if (windowWidth <= 768) {
              return;
            }

            gsap.to(img, {
              x: 0,
              y: 0,
              duration: 0.3,
              onComplete: () => {
                img.removeAttribute('style');
              }
            });
          }, false);
        }
      });
    }

    // All sections that require carousel functionality
    if (sections.length > 0) {
      sections.forEach(section => {
        const numShown = section.getAttribute('data-num');
        const contentWidth = section.querySelector('.sectionContent').getBoundingClientRect().width;
        const controls = Array.from(section.querySelectorAll('.sectionContent-controls > button.sectionContent-control'));
        const children = Array.from(section.querySelectorAll('.sectionContent-items .sectionItem'));
        let isMobile = windowWidth <= 768;

        if (children.length > numShown) {
          let flexsliderInstance = null;
          section.classList.remove('section-hideControls');

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

              // tl.staggerTo(children, 0.25, {
              //   left: isPrev ? 10 : -10
              // }, 0.1, '+=0', () => {
              //   console.log('done');
              //   tl.staggerTo(children, 0.25, {
              //     left: 0
              //   }, 0.1, '+=0');
              // })
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

        $(".trailerItem").colorbox({
          iframe: true,
          width: '70%',
          height: '70%'
        });
      });
    }
  }
});
