---
title: "Deploy Frontend & CloudFront CDN"
date: 2026-07-30
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

<!-- # DEPLOYING FRONTEND & CONFIGURING CLOUDFRONT CDN -->

In this section, we will deploy the **CodExecute React Frontend** application onto **Amazon S3** static storage, while integrating **Amazon CloudFront** to serve edge assets globally and route backend API calls to **Amazon API Gateway**.

#### Section Overview

<style>
#body a.toc-link, 
#body a.toc-link:hover, 
#body a.toc-link:focus, 
#body a.toc-link:active {
  border: 1px solid #e2e8f0 !important;
  border-bottom: 1px solid #e2e8f0 !important;
  text-decoration: none !important;
  background-image: none !important;
  box-shadow: 0 1px 3px rgba(0,0,0,0.04) !important;
}
#body a.toc-link:after, 
#body a.toc-link:before {
  display: none !important;
  content: "" !important;
}
#body a.toc-link:hover {
  border-color: #FF9900 !important;
  background-color: #FFFDF7 !important;
}
</style>

<div style="display: flex; flex-direction: column; gap: 10px; margin: 20px 0;">

  <a class="toc-link" href="./5.3.1-s3/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">5.3.1</span>
      <span style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Deploy Frontend to Amazon S3</span>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

  <a class="toc-link" href="./5.3.2-cf/" style="display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #ffffff; border-radius: 8px; color: inherit;">
    <div style="display: flex; align-items: center; gap: 14px;">
      <span style="font-family: monospace; font-size: 0.85rem; font-weight: 700; color: #FF9900; background: #FFF7ED; padding: 5px 10px; border-radius: 6px; border: 1px solid #FFEDD5;">5.3.2</span>
      <span style="font-weight: 700; font-size: 0.95rem; color: #1e293b;">Configure Amazon CloudFront &amp; Multi-Origin Routing</span>
    </div>
    <span style="font-size: 1.1rem; color: #cbd5e1; font-weight: 700;">&rarr;</span>
  </a>

</div>