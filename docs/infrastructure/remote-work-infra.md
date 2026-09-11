# Firebase Remote Config (Comprehensive Blueprint)
This document simulates the keys and default values that should be configured inside the Firebase Console under **Remote Config**. The Flutter app will fetch these values on startup to dynamically alter its behavior without requiring an app store update.

```json
{
  "core_endpoints": {
    "api_base_url": "https://api.crazycoconut.com/v1",
    "cdn_base_url": "https://cdn.crazycoconut.com"
  },
  
  "app_lifecycle": {
    "min_app_version_ios": "1.0.5",
    "min_app_version_android": "1.0.5",
    "maintenance_mode": false,
    "maintenance_message": "Our servers are currently undergoing scheduled maintenance to serve you better. We'll be back in 30 minutes!",
    "force_update_url_ios": "https://apps.apple.com/app/crazycoconut/id123456789",
    "force_update_url_android": "https://play.google.com/store/apps/details?id=com.crazycoconut.app",
    "show_app_rating_prompt_after_orders": 3
  },

  "seasonal_themes": {
    "current_theme_name": "diwali_festival",
    "dynamic_app_icon_name": "icon_diwali", 
    "splash_screen_animation_url": "https://cdn.crazycoconut.com/assets/lottie/splash_diwali.json",
    "empty_cart_animation_url": "https://cdn.crazycoconut.com/assets/lottie/empty_cart_diwali.json",
    "primary_color_hex": "#FF9800",
    "secondary_color_hex": "#E91E63",
    "confetti_animation_on_checkout": true
  },

  "dynamic_layouts": {
    "home_screen_section_order": ["banners", "categories", "flash_sale", "recommended_products"],
    "product_page_layout": "gallery_top_details_bottom"
  },

  "ab_testing_and_experiments": {
    "checkout_flow_variant": "one_click_checkout",
    "onboarding_flow_variant": "skip_allowed",
    "free_shipping_threshold": 499
  },

  "marketing_and_promotions": {
    "show_home_banner": true,
    "home_banner_image_url": "https://cdn.crazycoconut.com/assets/banners/diwali_mega_sale.png",
    "home_banner_action_url": "crazycoconut://category/diwali-sale",
    "enable_scratch_card_on_order": true,
    "referral_bonus_amount": 50,
    "show_abandoned_cart_popup": true
  },

  "feature_flags": {
    "enable_otp_login": true,
    "enable_google_login": true,
    "enable_apple_login": false,
    "enable_guest_checkout": false,
    "enable_upi_payments": true,
    "enable_wallet_payments": false,
    "enable_ar_preview": false,
    "enable_product_video_reviews": true
  },

  "support_and_legal": {
    "support_email": "support@crazycoconut.com",
    "support_phone": "+919876543210",
    "whatsapp_support_number": "+919876543210",
    "terms_url": "https://crazycoconut.com/terms",
    "privacy_url": "https://crazycoconut.com/privacy",
    "faq_url": "https://crazycoconut.com/faq",
    "return_policy_url": "https://crazycoconut.com/returns"
  },

  "app_ux_and_navigation": {
    "show_onboarding_screens": true,
    "default_start_tab": "home",
    "enable_bottom_nav_labels": true,
    "max_image_cache_size_mb": 250,
    "api_timeout_seconds": 15,
    "pull_to_refresh_enabled": true
  },

  "analytics_and_performance": {
    "enable_crashlytics": true,
    "analytics_sample_rate": 1.0,
    "log_network_requests": false,
    "enable_performance_monitoring": true
  },

  "payment_gateways": {
    "payment_environment": "production",
    "razorpay_key_public": "rzp_live_xxxxxxxxxxxxx",
    "razorpay_theme_color": "#FF9800",
    "stripe_publishable_key": "pk_live_xxxxxxxxxxxxx",
    "currency_code": "INR",
    "currency_symbol": "₹",
    "max_cod_amount": 5000,
    "cod_fee": 50
  }
}
```