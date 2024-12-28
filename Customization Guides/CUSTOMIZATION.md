# Najahi Portfolio Template - Customization Guide

This guide will help you customize your Najahi portfolio template. Follow the sections below to personalize your portfolio.

## Portfolio Customization

### 1. Header Customization (header.tsx)

- Replace logo images in the `/public/logo/` directory:
  - `najahi-dark.png` - Logo for dark theme
  - `najahi-light.png` - Logo for light theme
- Recommended logo dimensions: 120x40px

### 2. Hero Section (hero.tsx)

- Replace profile image:
  - Add your photo to `/public/Photo.png`
  - Recommended size: 200x200px, square format
- Update personal details:
  - Name: Replace "Mohammad Saiful Islam"
  - Designation: Replace "Founder & Partner"
- Update social links:
  - LinkedIn: Update `linkedin.com/in/yourusername`
  - GitHub: Update `github.com/yourusername`
  - Email: Update `your.email@example.com`
- Replace resume:
  - Add your resume to `/public/Resume.pdf`

### 3. About Section (about.tsx)

- Replace the personal profile statement in the `<motion.p>` component
- Remove or modify the demo content note

### 4. Experience Section (experience.tsx)

- Modify the `experiences` array:
  ```typescript
  {
    title: "Your Job Title",
    company: "Company Name",
    duration: "Start - End Date",
    responsibilities: [
      "Responsibility 1",
      "Responsibility 2",
      "Responsibility 3"
    ]
  }
  ```

### 5. Education Section (education.tsx)

- Update `educations` array with your educational background
- Update `certifications` array with your certifications
- Follow the existing format:
  ```typescript
  {
    degree: "Your Degree",
    subject: "Your Subject",
    cpga: "Your CGPA",
    institution: "Institution Name",
    year: "Completion Year"
  }
  ```

### 6. Skills Section (skills.tsx)

- Modify the `skills` array with your technical skills
- Add or remove skills as needed

### 7. Core Expertise Section (core-expertise.tsx)

- Update `expertiseAreas` array with your areas of expertise
- For each area, include:
  - Title
  - Description
  - Key achievements

### 8. Projects Section (projects.tsx)

- Update `projects` array with your notable projects
- For each project, include:
  - Title
  - Description
  - Technologies used
  - Project link

### 9. LinkedIn Posts Section (linkedin-posts.tsx)

- Configure LinkedIn API integration in `/api/linkedin-posts`
- Update LinkedIn username in the API configuration

### 10. Contact Section (contact.tsx)

- Update contact information:
  - Email address
  - Phone number
  - LinkedIn profile
  - GitHub profile
- Configure Cloudflare Turnstile:
  - Add NEXT_PUBLIC_CLOUDFLARE_TURNSTILE_SITE_KEY to your environment variables

### 11. Footer Section (footer.tsx)

- Update social media links
- Modify copyright text if needed

### 12. Theme Customization

#### Customize Global Styles (globals.css)

#### Light Theme Colors (ShadCN UI)

```css
:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  --primary: 221.2 83.2% 53.3%; /* Customize your primary brand color */
  --secondary: 210 40% 96.1%; /* Customize your secondary color */
  --accent: 210 40% 96.1%; /* Customize your accent color */
  /* Modify other color variables as needed */
}
```

#### Dark Theme Colors (ShadCN UI)

```css
.dark {
  --background: 222.2 84% 4.9%;
  --foreground: 210 40% 98%;
  --primary: 217.2 91.2% 59.8%; /* Customize dark theme primary color */
  --secondary: 217.2 32.6% 17.5%; /* Customize dark theme secondary */
  /* Modify other dark theme colors as needed */
}
```

### 13. Analytics Setup (Analytics.tsx)

#### Required Environment Variables

Create `.env` file from `.env.example`:

```env
# Google Analytics
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX

# Microsoft Bing
NEXT_PUBLIC_BING_ID=XXXXXXXXXX

# Yandex Metrica
NEXT_PUBLIC_YANDEX_ID=XXXXXXXXXX
```

#### Contact Form Configuration

```env
# Email Settings
EMAIL_USER=your@email.com
EMAIL_PASS=your-email-password
EMAIL_TO=recipient@email.com

# Cloudflare Turnstile
NEXT_PUBLIC_CLOUDFLARE_TURNSTILE_SITE_KEY=your-site-key
CLOUDFLARE_TURNSTILE_SECRET_KEY=your-secret-key
```

#### Security Settings

```env
# CORS Configuration
CORS_ORIGINS=https://yourdomain.com,https://api.yourdomain.com
NODE_ENV=production
```

### 14. SEO Customization

#### Update Metadata Base (layout.tsx)

```typescript
export const metadata: Metadata = {
  metadataBase: new URL("https://yourdomain.com"), // Your actual domain
  title: {
    default: "Your Name - Portfolio",
    template: "%s | Your Professional Title",
  },
  description: "Your professional summary in 150-160 characters",
  keywords: ["your", "relevant", "keywords"],
  authors: [{ name: "Your Name" }],
  creator: "Your Name/Company",
};
```

#### OpenGraph Settings (layout.tsx)

```typescript
openGraph: {
  type: "website",
  url: "https://yourdomain.com",
  siteName: "Your Name - Portfolio",
  images: [{
    url: "https://yourdomain.com/og-image.jpg",
    width: 1200,
    height: 630,
    alt: "Your Portfolio Preview"
  }]
}
```

#### Twitter Cards (layout.tsx)

```typescript
twitter: {
  card: "summary_large_image",
  creator: "@yourhandle",
  images: ["https://yourdomain.com/twitter-image.jpg"]
}
```

#### Page-Specific SEO (page.tsx)

```typescript
export const metadata: Metadata = {
  title: "Your Name - Professional Title",
  description: "Your professional summary focused on your expertise",
  openGraph: {
    title: "Your Name - Professional Title",
    description: "Compelling description for social sharing",
  },
  twitter: {
    title: "Your Name - Professional Title",
    description: "Engaging description for Twitter shares",
  },
};
```

#### Important Notes for SEO Customization

1. All image URLs should be absolute paths
2. Test metadata using social media debugging tools
3. Verify analytics tracking is working
4. Ensure environment variables are set in production
5. Optimize images for social sharing
6. Test contact form functionality
7. Monitor CORS settings for API endpoints

### 15. Important Notes for Portfolio Customization

- Optimize image sizes for better performance.
- Test all links before deployment.
- Ensure personal information is accurate.
- Update and secure environment variables in the deployment platform.
- Test contact form functionality after customization.
- Replace placeholder content with actual information.
- Test the app across devices and browsers.
- Validate structured data using Google's testing tool.
- Monitor analytics after deployment.
