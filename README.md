# Experiment-15

# Image Carousel in react using hooksAssignment

## Aim:
To create image carousel in react using hooksAssignment.

## Algorithm:
1.Start React project,using "create-react-app projectName".

2.To create the folder,go to the folder using "code .".

3.To start the website using localhost,npm start.

4.Edit the components in the folder,for the project.

5.Edit App.js and enter the HTML code in the return and render.
## Program
### index.html
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="//fonts.googleapis.com/css?family=Raleway:300" rel="stylesheet">
    <link rel="stylesheet" href="/dist/image-gallery.css"/>
    <title>React Image Gallery</title>
  </head>
  <body>
    <div id="root" class="gallery-container"></div>
    <script src="/dist/example.js"></script>
  </body>
</html>
```
### app.css
```
body {
  margin: 0;
  padding: 0;
  background: #f4f6f9;
  color: #222;
  font-family: 'Raleway', sans-serif;
}

ul, li {
  padding: 0;
  margin: 0;
  list-style: none;
}

li {
  padding: 3px 0;
  display: inline-block;
}

label {
  margin-left: 5px;
}

.gallery-container {
  margin-top: 50px;
}

.app-header {
  letter-spacing: 1px;
  text-transform: uppercase;
}

.play-button {
  cursor: pointer;
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  height: 60px;
  width: 100px;
  background-color: rgba(0, 0, 0, 0.7);
  border-radius: 5px;
}

.play-button:hover {
  background-color: rgba(0, 0, 0, 0.9);
}

.play-button:after {
  content: "";
  display: block;
  position: absolute;
  top: 16.5px;
  left: 40px;
  margin: 0 auto;
  border-style: solid;
  border-width: 12.5px 0 12.5px 20px;
  border-color: transparent transparent transparent rgba(255, 255, 255, 1);
}

.close-video::before {
  content: '✖';
  cursor: pointer;
  position: absolute;
  right: 0;
  top: 0;
  font-size: 20px;
  padding: 20px;
  z-index: 1;
  line-height: .7;
  display: block;
  color: #fff;
}

.video-wrapper {
  position: relative;
  padding: 33.35% 0; /* 16:9 */
  height: 0;
}

.video-wrapper iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}


.app .image-gallery,
.app-sandbox {
  margin: 0 auto;
  width: 65%;
}


@media (max-width: 1320px) {
  .app-sandbox-content {
    padding: 0 20px;
  }
}

.app-sandbox {
  margin: 40px auto;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}

.app-buttons li {
  display: block;
}

@media (max-width: 768px) {

  .app-header {
    font-size: 20px;
  }

  .app-buttons li {
    display: block;
    margin: 10px 0;
  }

  .app-buttons li + li {
    padding: 0;
  }

  .play-button {
    height: 40px;
    width: 65px;
  }

  .play-button:after {
    top: 11px;
    left: 27px;
    border-width: 8.5px 0 8.5px 12px;
  }

  .close-video::before {
    font-size: 16px;
    padding: 15px;
  }
}

@media (max-width: 1024px) {
  .app .image-gallery,
  .app-sandbox {
    width: 100%;
  }
}

.app-interval-input-group {
  display: table;
}

.app-interval-label {
  display: table-cell;
  vertical-align: middle;
  padding: 6px 12px;
  font-size: 14px;
  font-weight: 400;
  line-height: 1;
  color: #555;
  text-align: center;
  background-color: #eee;
  border: 3px solid #ccc;
  border-right: none;
  border-radius: 4px;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

.app-interval-input {
  -webkit-appearance: none;
  display: table-cell;
  margin: 0;
  padding: 9px;
  border-radius: 5px;
  font-size: 14px;
  border: 3px solid #ccc;
  background: #fff;
  width: 100%;
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
}

input.app-interval-input {
  width: 65px;
}

.app-checkboxes {
  margin-top: 10px;
}

.app-checkboxes li {
  display: block;
}
```
### app.js
```
import React from 'react';
import { createRoot } from 'react-dom/client';

import ImageGallery from 'src/ImageGallery';

const PREFIX_URL = 'https://raw.githubusercontent.com/xiaolin/react-image-gallery/master/static/';


class App extends React.Component {

  constructor() {
    super();
    this.state = {
      showIndex: false,
      showBullets: true,
      infinite: true,
      showThumbnails: true,
      showFullscreenButton: true,
      showGalleryFullscreenButton: true,
      showPlayButton: true,
      showGalleryPlayButton: true,
      showNav: true,
      isRTL: false,
      slideDuration: 450,
      slideInterval: 2000,
      slideOnThumbnailOver: false,
      thumbnailPosition: 'bottom',
      showVideo: {},
      useWindowKeyDown: true,
    };

    this.images = [
      {
        thumbnail: `${PREFIX_URL}4v.jpg`,
        original: `${PREFIX_URL}4v.jpg`,
        embedUrl: 'https://www.youtube.com/embed/4pSzhZ76GdM?autoplay=1&showinfo=0',
        description: 'Render custom slides (such as videos)',
        renderItem: this._renderVideo.bind(this)
      },
      {
        original: `${PREFIX_URL}1.jpg`,
        thumbnail: `${PREFIX_URL}1t.jpg`,
        originalClass: 'featured-slide',
        thumbnailClass: 'featured-thumb',
        description: 'Custom class for slides & thumbnails',
      },
    ].concat(this._getStaticImages());
  }

  _onImageClick(event) {
    console.debug('clicked on image', event.target, 'at index', this._imageGallery.getCurrentIndex());
  }

  _onImageLoad(event) {
    console.debug('loaded image', event.target.src);
  }

  _onSlide(index) {
    this._resetVideo();
    console.debug('slid to index', index);
  }

  _onPause(index) {
    console.debug('paused on index', index);
  }

  _onScreenChange(fullScreenElement) {
    console.debug('isFullScreen?', !!fullScreenElement);
  }

  _onPlay(index) {
    console.debug('playing from index', index);
  }

  _handleInputChange(state, event) {
    if (event.target.value > 0) {
      this.setState({[state]: event.target.value});
    }
  }

  _handleCheckboxChange(state, event) {
    this.setState({[state]: event.target.checked});
  }

  _handleThumbnailPositionChange(event) {
    this.setState({thumbnailPosition: event.target.value});
  }

  _getStaticImages() {
    let images = [];
    for (let i = 2; i < 12; i++) {
      images.push({
        original: `${PREFIX_URL}${i}.jpg`,
        thumbnail:`${PREFIX_URL}${i}t.jpg`
      });
    }

    return images;
  }

  _resetVideo() {
    this.setState({showVideo: {}});

    if (this.state.showPlayButton) {
      this.setState({showGalleryPlayButton: true});
    }

    if (this.state.showFullscreenButton) {
      this.setState({showGalleryFullscreenButton: true});
    }
  }

  _toggleShowVideo(url) {
    const showVideo = this.state;
    this.setState({
      showVideo: {
        ...showVideo,
        [url]: !showVideo[url]
      }
    });

    if (!showVideo[url]) {
      if (this.state.showPlayButton) {
        this.setState({showGalleryPlayButton: false});
      }

      if (this.state.showFullscreenButton) {
        this.setState({showGalleryFullscreenButton: false});
      }
    }
  }

  _renderVideo(item) {
    return (
      <div>
        {
          this.state.showVideo[item.embedUrl] ?
            <div className='video-wrapper'>
                <a
                  className='close-video'
                  onClick={this._toggleShowVideo.bind(this, item.embedUrl)}
                >
                </a>
                <iframe
                  width='560'
                  height='315'
                  src={item.embedUrl}
                  frameBorder='0'
                  allowFullScreen
                >
                </iframe>
            </div>
          :
            <a onClick={this._toggleShowVideo.bind(this, item.embedUrl)}>
              <div className='play-button'></div>
              <img className='image-gallery-image' src={item.original} />
              {
                item.description &&
                  <span
                    className='image-gallery-description'
                    style={{right: '0', left: 'initial'}}
                  >
                    {item.description}
                  </span>
              }
            </a>
        }
      </div>
    );
  }

  render() {
    return (
      <section className='app'>
        <ImageGallery
          ref={i => this._imageGallery = i}
          items={this.images}
          onClick={this._onImageClick.bind(this)}
          onImageLoad={this._onImageLoad}
          onSlide={this._onSlide.bind(this)}
          onPause={this._onPause.bind(this)}
          onScreenChange={this._onScreenChange.bind(this)}
          onPlay={this._onPlay.bind(this)}
          infinite={this.state.infinite}
          showBullets={this.state.showBullets}
          showFullscreenButton={this.state.showFullscreenButton && this.state.showGalleryFullscreenButton}
          showPlayButton={this.state.showPlayButton && this.state.showGalleryPlayButton}
          showThumbnails={this.state.showThumbnails}
          showIndex={this.state.showIndex}
          showNav={this.state.showNav}
          isRTL={this.state.isRTL}
          thumbnailPosition={this.state.thumbnailPosition}
          slideDuration={parseInt(this.state.slideDuration)}
          slideInterval={parseInt(this.state.slideInterval)}
          slideOnThumbnailOver={this.state.slideOnThumbnailOver}
          additionalClass="app-image-gallery"
          useWindowKeyDown={this.state.useWindowKeyDown}
        />

        <div className='app-sandbox'>

          <div className='app-sandbox-content'>
            <h2 className='app-header'>Settings</h2>

            <ul className='app-buttons'>
              <li>
                <div className='app-interval-input-group'>
                  <span className='app-interval-label'>Play Interval</span>
                  <input
                    className='app-interval-input'
                    type='text'
                    onChange={this._handleInputChange.bind(this, 'slideInterval')}
                    value={this.state.slideInterval}/>
                </div>
              </li>

              <li>
                <div className='app-interval-input-group'>
                  <span className='app-interval-label'>Slide Duration</span>
                  <input
                    className='app-interval-input'
                    type='text'
                    onChange={this._handleInputChange.bind(this, 'slideDuration')}
                    value={this.state.slideDuration}/>
                </div>
              </li>

              <li>
                <div className='app-interval-input-group'>
                  <span className='app-interval-label'>Thumbnail Bar Position</span>
                  <select
                    className='app-interval-input'
                    value={this.state.thumbnailPosition}
                    onChange={this._handleThumbnailPositionChange.bind(this)}
                  >
                    <option value='bottom'>Bottom</option>
                    <option value='top'>Top</option>
                    <option value='left'>Left</option>
                    <option value='right'>Right</option>
                  </select>
                </div>
              </li>
            </ul>

            <ul className='app-checkboxes'>
              <li>
                <input
                  id='infinite'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'infinite')}
                  checked={this.state.infinite}/>
                  <label htmlFor='infinite'>allow infinite sliding</label>
              </li>
              <li>
                <input
                  id='show_fullscreen'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'showFullscreenButton')}
                  checked={this.state.showFullscreenButton}/>
                  <label htmlFor='show_fullscreen'>show fullscreen button</label>
              </li>
              <li>
                <input
                  id='show_playbutton'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'showPlayButton')}
                  checked={this.state.showPlayButton}/>
                  <label htmlFor='show_playbutton'>show play button</label>
              </li>
              <li>
                <input
                  id='show_bullets'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'showBullets')}
                  checked={this.state.showBullets}/>
                  <label htmlFor='show_bullets'>show bullets</label>
              </li>
              <li>
                <input
                  id='show_thumbnails'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'showThumbnails')}
                  checked={this.state.showThumbnails}/>
                  <label htmlFor='show_thumbnails'>show thumbnails</label>
              </li>
              <li>
                <input
                  id='show_navigation'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'showNav')}
                  checked={this.state.showNav}/>
                  <label htmlFor='show_navigation'>show navigation</label>
              </li>
              <li>
                <input
                  id='show_index'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'showIndex')}
                  checked={this.state.showIndex}/>
                  <label htmlFor='show_index'>show index</label>
              </li>
              <li>
                <input
                  id='is_rtl'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'isRTL')}
                  checked={this.state.isRTL}/>
                  <label htmlFor='is_rtl'>is right to left</label>
              </li>
              <li>
                <input
                  id='slide_on_thumbnail_hover'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'slideOnThumbnailOver')}
                  checked={this.state.slideOnThumbnailOver}/>
                  <label htmlFor='slide_on_thumbnail_hover'>slide on mouse over thumbnails</label>
              </li>
              <li>
                <input
                  id='use_window_keydown'
                  type='checkbox'
                  onChange={this._handleCheckboxChange.bind(this, 'useWindowKeyDown')}
                  checked={this.state.useWindowKeyDown}/>
                  <label htmlFor='use_window_keydown'>use window keydown</label>
              </li>
            </ul>
          </div>

        </div>
      </section>
    );
  }
}

const container = document.getElementById('root');
const root = createRoot(container);
root.render(<App />);
```
## Output:
![image](https://github.com/SOMEASVAR/Image-Carousel/assets/93434149/49b36899-26ee-4e6e-b3a3-219cb94b248b)




## Result:
Therefore we have succesfully created a image carousel in react using hooksAssignment.

