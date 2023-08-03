# Infinite scroll

### Example with intersection observer (do not support in old browsers, see the "Can i use" resource)

This component is an Infinite Scroll component in React. It takes the following props: children (the content to be rendered), loading (a boolean indicating whether more data is currently being fetched), and fetchData (a function to fetch additional data).

When the component mounts, it immediately calls the fetchData function to fetch the initial set of data. The component uses the Intersection Observer API to observe a specific container element (defined by the containerRef ref) as the user scrolls down the page.

When the observed element (the intersection target) intersects with the viewport by a threshold of 10%, and the loading state is false, the handleIntersection function is triggered. In response, the fetchData function is called again to fetch more data, which is then appended to the existing content.

The component provides a smooth infinite scroll experience by dynamically loading new content as the user scrolls, leading to a seamless and engaging browsing experience for the user.

```ts
import { FC, useState, useEffect, useRef } from "react";

interface Props {
  loading: boolean;
  fetchData: () => void;
}

export const InfiniteScroll: FC<Props> = ({ children, loading, fetchData }) => {
  const containerRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    // Fetch initial data when the component mounts
    fetchData();
  }, []);

  const handleIntersection: IntersectionObserverCallback = (entries) => {
    const entry = entries[0];
    if (entry.isIntersecting && !loading) {
      fetchData();
    }
  };

  useEffect(() => {
    const options: IntersectionObserverInit = {
      root: null,
      rootMargin: "0px",
      threshold: 0.1, // Set a threshold value to trigger the intersection callback
    };

    const observer = new IntersectionObserver(handleIntersection, options);

    if (containerRef.current) {
      observer.observe(containerRef.current);
    }

    return () => observer.disconnect();
  }, [loading]); // Add `loading` to dependency array to avoid re-adding the observer

  return (
    <div>
      {children}
      <div ref={containerRef} style={{ height: "1px" }} /> {/* Intersection target */}
      {loading && <div>Loading...</div>}
    </div>
  );
};
```

### If you need support in old browsers you can use implementation with listening scroll

The same logic of implementation but with listening scroll

```ts
import { FC, useState, useEffect } from "react";

interface Props {
  loading: boolean;
  fetchData: () => void;
}

const InfiniteScrollComponent: FC<Props> = ({
  children,
  loading,
  fetchData,
}) => {
  useEffect(() => {
    // Fetch initial data when the component mounts
    fetchData();
  }, []);

  const handleScroll = () => {
    const threshold = 100; // Threshold in pixels before loading more data
    const scrollPosition = window.innerHeight + window.scrollY;
    const contentHeight = document.body.offsetHeight;

    if (contentHeight - scrollPosition < threshold && !loading) {
      fetchData();
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", handleScroll);
    return () => window.removeEventListener("scroll", handleScroll);
  }, [loading]); // Add `loading` to dependency array to avoid re-adding the event listener

  return (
    <div>
      {children}
      {loading && <div>Loading...</div>}
    </div>
  );
};
```
