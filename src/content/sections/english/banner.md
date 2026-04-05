---
# For Updating Contact Info Like Email, Phone Number, Address, etc. Please update in `src/config/config.toml` `settings.contactInfo` table

enable: true # Control the visibility of this section across all pages where it is used
titleSize: "display-4" # If your title text is larger, use a smaller text size like "display-3", "display-2", or "display-1".
title: "<span class='text-xl md:text-5xl'>  Plan <span class='text-[#E4FF02] mx-2'>▶︎</span>Build <span class='text-[#E4FF02] mx-2'>▶︎</span>Ship <span class='text-[#E4FF02] mx-2'>▶︎</span>Scale</span>"

image: "/images/banner/startup.png"
description: 'We turn ideas into <span class="font-semibold text-[#E4FF02]">scalable software</span>— from <span class="font-medium text-gray-200">MVP to production</span>, <br>built to handle real users and growth..'

button:
  # Refer to the `sharedButton` schema in `src/sections.schema.ts` for all available configuration options (e.g., enable, label, url, hoverEffect, variant, icon, tag, rel, class, target, etc.)
  enable: false
  label: "VIEW OUR WORKS"
  url: "/"
  # hoverEffect: "" # Optional: text-flip | creative-fill | magnetic | magnetic-text-flip
  # variant: "" # Optional: fill | outline | text | circle
  # rel: "" # Optional
  # target: "" # Optional
---
