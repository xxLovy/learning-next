---
title: Next
date: 2024-04-23 10:10:01
tags:
---







`clsx` for conditionally styling

```tsx
import clsx from 'clsx';
 
export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
    // ...
)}
```

## Fonts in Next.js

Next.js downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.

1. Create font.ts in `app/ui`
2. use other fonts

```ts
import { Inter, Lusitana } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });

export const lusitana = Lusitana({
    weight: ['400', '700'],
    subsets: ['latin'],
});
```

3. Apply font throughout the application

```tsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

```tsx
import { inter } from '@/app/ui/fonts';
...
      <body className={`${inter.className} antialiased`}>{children}</body>
```

Add Tailwind [`antialiased`](https://tailwindcss.com/docs/font-smoothing) class to smooths out the font. It's not necessary to use this class.

## Image in Next.js

With regular HTML, you have to manually:

- Ensure your image is responsive on different screen sizes.
- Specify image sizes for different devices.
- Prevent layout shift as the images load.
- Lazy load images that are outside the user's viewport.

Image Optimization is a large topic in web development that could be considered a specialization in itself. Instead of manually implementing these optimizations, you can use the `next/image` component to automatically optimize your images.

### [The `Image` component](https://nextjs.org/learn/dashboard-app/optimizing-fonts-images#the-image-component)

The `<Image>` Component is an extension of the HTML `<img>` tag, and comes with automatic image optimization, such as:

- Preventing layout shift automatically when images are loading.
- Resizing images to avoid shipping large images to devices with a smaller viewport.
- Lazy loading images by default (images load as they enter the viewport).
- Serving images in modern formats, like [WebP](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#webp) and [AVIF](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#avif_image), when the browser supports it.

```tsx
import Image from 'next/image';
 
export default function Page() {
  return (
    ...
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
    ...
```

## Routing in Next.js

### `page.tsx`

`page.tsx` is a special Next.js file that exports a React component, and it's required for the route to be accessible.

nested folder in apps with `page.tsx` means nested routers.

For example, when a fold structure is like this

<img src="/Users/xuxuan/Library/Application Support/typora-user-images/image-20240425173041003.png" alt="image-20240425173041003" style="zoom:33%;" />

`localhost:3000` renders `page.tsx` in app folder:

<img src="/Users/xuxuan/Library/Application Support/typora-user-images/image-20240425173136328.png" alt="image-20240425173136328" style="zoom:33%;" />

`localhost:3000/dashboard` renders `page.tsx` in dashboard folder:

<img src="/Users/xuxuan/Library/Application Support/typora-user-images/image-20240425173200583.png" alt="image-20240425173200583" style="zoom:33%;" />

`localhost:3000/dashboard/customers `renders `page.tsx` in customers folder:

<img src="/Users/xuxuan/Library/Application Support/typora-user-images/image-20240425173232817.png" alt="image-20240425173232817" style="zoom:33%;" />

### `layout.tsx`

In Next.js, you can use a special `layout.tsx` file to create UI that is shared between multiple pages. Let's create a layout for the dashboard pages!

create a `layout.tsx` in `/app/dashboard`

<img src="/Users/xuxuan/Library/Application Support/typora-user-images/image-20240425173612899.png" alt="image-20240425173612899" style="zoom:33%;" />

The pages inside `/app/dashboard` will automatically be nested inside a `<Layout />`.

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called [partial rendering](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#3-partial-rendering)

# Navigating Between Pages

In Next.js, you can use the `<Link />` Component to link between pages in your application. `<Link>` allows you to do [client-side navigation](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#how-routing-and-navigation-works) with JavaScript.

To improve the navigation experience, Next.js automatically code splits your application by route segments.

