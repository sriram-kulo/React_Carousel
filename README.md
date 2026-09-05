# Ex05 Image Carousel
## Date: 05-09-2026

## AIM
To create an Image Carousel using React.

## ALGORITHM
### STEP 1 Initial Setup:
Input: A list of images to display in the carousel.

Output: A component displaying the images with navigation controls (e.g., next/previous buttons).

### Step 2 State Management:
Use a state variable (`currentIndex`) to track the index of the current image displayed.

The carousel starts with the first image, so initialize `currentIndex` to 0.

### Step 3 Navigation Controls:
Next Image: When the "Next" button is clicked, increment `currentIndex`.

If `currentIndex` is at the end of the image list (last image), loop back to the first image using modulo:
`currentIndex = (currentIndex + 1) % images.length;`

Previous Image: When the "Previous" button is clicked, decrement `currentIndex`.

If `currentIndex` is at the beginning (first image), loop back to the last image:
`currentIndex = (currentIndex - 1 + images.length) % images.length;`

### Step 4 Displaying the Image:
The `currentIndex` determines which image is displayed.

Using the `currentIndex`, display the corresponding image from the images list.

### Step 5 Auto-Rotation:
Set an interval to automatically change the image after a set amount of time (e.g., 3 seconds).

Use `setInterval` to call the `nextImage()` function at regular intervals.

Clean up the interval when the component unmounts using `clearInterval` to prevent memory leaks.

## PROGRAM

### src/App.jsx
```jsx
import React, { useState, useEffect, useCallback } from 'react';
import './App.css';

const images = [
  {
    url: '[https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=1000&q=80](https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=1000&q=80)',
    title: 'Yosemite Valley',
    description: 'Breathtaking view of granite cliffs and misty pine trees.'
  },
  {
    url: '[https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?auto=format&fit=crop&w=1000&q=80](https://images.unsplash.com/photo-1470071459604-3b5ec3a7fe05?auto=format&fit=crop&w=1000&q=80)',
    title: 'Misty Wilderness',
    description: 'Fog creeping over alpine mountains and lush ridges.'
  },
  {
    url: '[https://images.unsplash.com/photo-1441974231531-c6227db76b6e?auto=format&fit=crop&w=1000&q=80](https://images.unsplash.com/photo-1441974231531-c6227db76b6e?auto=format&fit=crop&w=1000&q=80)',
    title: 'Sunlit Forest',
    description: 'Golden morning light filtering through tall woodland trees.'
  },
  {
    url: '[https://images.unsplash.com/photo-1472214103451-9374bd1c798e?auto=format&fit=crop&w=1000&q=80](https://images.unsplash.com/photo-1472214103451-9374bd1c798e?auto=format&fit=crop&w=1000&q=80)',
    title: 'Green Meadows',
    description: 'Vast rolling hills bathed in warm afternoon sunlight.'
  }
];

export default function App() {
  // Step 2: State variable to track the index of the current image
  const [currentIndex, setCurrentIndex] = useState(0);

  // Step 3: Next Image with loop back using modulo
  const nextImage = useCallback(() => {
    setCurrentIndex((prev) => (prev + 1) % images.length);
  }, []);

  // Step 3: Previous Image with loop back to last image
  const prevImage = () => {
    setCurrentIndex((prev) => (prev - 1 + images.length) % images.length);
  };

  // Step 5: Auto-Rotation every 3 seconds and cleanup on unmount
  useEffect(() => {
    const timer = setInterval(() => {
      nextImage();
    }, 3000);

    return () => clearInterval(timer); // Prevent memory leaks
  }, [nextImage]);

  return (
    <div className="carousel-wrapper">
      <h2 className="carousel-title">React Image Carousel</h2>
      
      <div className="carousel-container">
        {/* Step 4: Displaying the Current Image */}
        <div className="carousel-slide">
          <img
            src={images[currentIndex].url}
            alt={images[currentIndex].title}
            className="carousel-image"
          />
          <div className="carousel-caption">
            <h3>{images[currentIndex].title}</h3>
            <p>{images[currentIndex].description}</p>
          </div>
        </div>

        {/* Step 3: Navigation Controls */}
        <button 
          className="nav-btn prev-btn" 
          onClick={prevImage} 
          aria-label="Previous Slide"
        >
          &#10094;
        </button>
        <button 
          className="nav-btn next-btn" 
          onClick={nextImage} 
          aria-label="Next Slide"
        >
          &#10095;
        </button>

        {/* Dot Indicators */}
        <div className="carousel-dots">
          {images.map((_, index) => (
            <span
              key={index}
              className={`dot ${currentIndex === index ? 'active' : ''}`}
              onClick={() => setCurrentIndex(index)}
            />
          ))}
        </div>
      </div>
    </div>
  );
}
```

### src/App.css
```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  background-color: #f1f5f9;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

.carousel-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 2rem;
}

.carousel-title {
  font-size: 2.2rem;
  color: #0f172a;
  margin-bottom: 1.8rem;
  font-weight: 700;
}

.carousel-container {
  position: relative;
  width: 100%;
  max-width: 800px;
  height: 460px;
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 20px 30px -10px rgba(0, 0, 0, 0.2);
  background-color: #000;
}

.carousel-slide {
  position: relative;
  width: 100%;
  height: 100%;
}

.carousel-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.carousel-caption {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1.5rem;
  background: linear-gradient(180deg, transparent 0%, rgba(0, 0, 0, 0.8) 100%);
  color: #ffffff;
  text-align: left;
}

.carousel-caption h3 {
  font-size: 1.4rem;
  margin-bottom: 0.3rem;
}

.carousel-caption p {
  font-size: 0.95rem;
  color: #cbd5e1;
}

.nav-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(255, 255, 255, 0.9);
  color: #0f172a;
  border: none;
  font-size: 1.4rem;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.25s ease;
  z-index: 10;
}

.nav-btn:hover {
  background-color: #ffffff;
  transform: translateY(-50%) scale(1.1);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
}

.prev-btn {
  left: 1rem;
}

.next-btn {
  right: 1rem;
}

.carousel-dots {
  position: absolute;
  bottom: 1rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 0.5rem;
  z-index: 10;
}

.dot {
  width: 11px;
  height: 11px;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.3s ease;
}

.dot.active {
  background-color: #ffffff;
  transform: scale(1.25);
}

@media (max-width: 640px) {
  .carousel-container {
    height: 320px;
  }
}
```

## OUTPUT

<img width="1521" height="963" alt="Screenshot 2026-09-05 135841" src="https://github.com/user-attachments/assets/0eab3863-5ed3-4f91-b04c-a27687f76f3a" />
<img width="1917" height="962" alt="Screenshot 2026-09-05 135830" src="https://github.com/user-attachments/assets/61a72eae-94a9-4aba-abb6-ae507438df48" />

## RESULT
The program for creating Image Carousel using React is executed successfully.
