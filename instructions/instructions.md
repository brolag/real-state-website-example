# UI/UX Guidelines for a Costa Rica Real Estate Website

If you're coding this from scratch, I'll break it down into design principles, structure, UI components, and best practices for a seamless UX.

## 1️⃣ General UX Principles

✔ Minimalist & Luxurious Design – Use whitespace, large imagery, and soft typography to reflect high-end real estate.  
✔ Fast Performance – Optimize images, use lazy loading, and ensure a smooth experience.  
✔ Mobile-First Approach – Ensure a responsive grid system (flexbox or CSS grid).  
✔ Clear Navigation – Users should find information within 3 clicks.  
✔ Strong CTAs – Buttons should stand out but not be overwhelming.

## 2️⃣ Structure & Components

Use Next.js (React framework) for fast performance and SEO-friendly static generation.

### 📌 Header & Navigation
- Fixed navbar with a transparent background on the homepage, turning solid on scroll.
- Include logo (left), menu (center/right), and CTA buttons (right).
- Hover effects on menu items.
- Sticky contact button (WhatsApp or form) at the bottom right.

```jsx
<header className="fixed top-0 left-0 w-full bg-transparent backdrop-blur-md transition-all shadow-sm">
  <nav className="flex justify-between items-center px-6 py-4">
    <a href="/" className="text-xl font-bold text-forest-green">Brand Logo</a>
    <ul className="flex gap-6 text-lg">
      <li><a href="/about" className="hover:text-sand">About</a></li>
      <li><a href="/services" className="hover:text-sand">Services</a></li>
      <li><a href="/sale" className="hover:text-sand">Sale</a></li>
      <li><a href="/rent" className="hover:text-sand">Rent</a></li>
      <li><a href="/contact" className="hover:text-sand">Contact</a></li>
    </ul>
  </nav>
</header>
```

### 📌 Hero Section
- Large full-width image or video (luxury real estate drone shot).
- Overlay text with a strong tagline + CTA button.
- CTA Button Example: Schedule a Visit → Modal or WhatsApp link.

```jsx
<section className="relative w-full h-screen flex items-center justify-center">
  <img src="/hero-image.jpg" className="absolute w-full h-full object-cover brightness-75" />
  <div className="relative text-center text-white">
    <h1 className="text-5xl font-bold mb-4">Find Your Dream Home in Costa Rica</h1>
    <p className="text-lg mb-6">Exclusive properties, expert advisory, proven results.</p>
    <a href="/contact" className="bg-forest-green text-white px-6 py-3 rounded-lg hover:bg-opacity-80 transition">Schedule a Visit</a>
  </div>
</section>
```

### 📌 About & Services
- Grid-based layout (2-column text + image).
- Minimalist cards with hover effects.
- Use scroll animations for smooth reveals.

```jsx
<section className="py-16 bg-soft-ivory">
  <div className="container mx-auto grid md:grid-cols-2 gap-12 items-center">
    <img src="/luxury-villa.jpg" className="rounded-2xl shadow-lg" />
    <div>
      <h2 className="text-4xl font-semibold text-charcoal mb-4">Our Expertise</h2>
      <p className="text-lg text-gray-600">We provide end-to-end real estate advisory...</p>
      <ul className="mt-6">
        <li className="flex items-center gap-2"><span className="text-green-500">✔</span> Luxury Property Sales</li>
        <li className="flex items-center gap-2"><span className="text-green-500">✔</span> Exclusive Rentals</li>
        <li className="flex items-center gap-2"><span className="text-green-500">✔</span> Investment Advisory</li>
      </ul>
    </div>
  </div>
</section>
```

### 📌 Properties Grid (Sale & Rent Pages)
- Display properties in a responsive card grid.
- Lazy load images to improve performance.
- Include price, location, and a CTA button (e.g., "View Property").

```jsx
<section className="py-16">
  <div className="container mx-auto grid md:grid-cols-3 gap-8">
    {properties.map((property) => (
      <div key={property.id} className="shadow-lg rounded-lg overflow-hidden">
        <img src={property.image} className="w-full h-60 object-cover" />
        <div className="p-4">
          <h3 className="text-xl font-bold">{property.title}</h3>
          <p className="text-gray-500">{property.location}</p>
          <p className="text-forest-green text-lg font-semibold">${property.price}</p>
          <a href={`/property/${property.id}`} className="mt-3 inline-block bg-forest-green text-white px-4 py-2 rounded-md">View Property</a>
        </div>
      </div>
    ))}
  </div>
</section>
```

### 📌 Contact & Footer
- Short lead capture form (Name, Email, Phone, Message).
- WhatsApp CTA for immediate inquiries.
- Social media & legal links in the footer.

```jsx
<footer className="bg-charcoal text-white py-12">
  <div className="container mx-auto flex flex-col md:flex-row justify-between">
    <div>
      <h4 className="text-lg font-bold">Get in Touch</h4>
      <p className="mt-2">📍 San José, Costa Rica</p>
      <p>📧 hello@realestatecr.com</p>
      <p>📞 +506 1234-5678</p>
    </div>
    <div>
      <h4 className="text-lg font-bold">Follow Us</h4>
      <div className="flex gap-4 mt-2">
        <a href="#" className="hover:text-sand">Facebook</a>
        <a href="#" className="hover:text-sand">Instagram</a>
        <a href="#" className="hover:text-sand">LinkedIn</a>
      </div>
    </div>
  </div>
</footer>
```

## 3️⃣ Best Practices for Development
- Use Tailwind CSS or Styled Components for maintainability.
- SEO Optimization:
  - Metadata & Open Graph tags for property listings.
  - Lazy Loading for images.
  - Schema Markup for real estate listings.
- Performance Enhancements:
  - Use Next.js Image Optimization.
  - Enable Server-Side Rendering (SSR) for property pages.
  - Implement a Content Delivery Network (CDN).

## 4️⃣ Tech Stack Recommendations

✔ Next.js + TypeScript → Best for performance and scalability.  
✔ Tailwind CSS → Fast and flexible styling.  
✔ Sanity.io / Strapi → Headless CMS for property management.  
✔ Cloudinary / Imgix → Optimize real estate images.  
✔ Mapbox / Google Maps API → Interactive property location maps.  
✔ HubSpot / Mailchimp → CRM for leads and follow-ups.

## Final Thoughts
- The goal is to create a high-end, luxurious experience while keeping performance in check.
- The combination of beautiful images, soft colors, and clear navigation will maximize conversions.
- Integration with WhatsApp & an intuitive contact form will enhance lead generation.