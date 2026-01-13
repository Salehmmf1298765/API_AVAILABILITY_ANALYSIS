# واجهات برمجة تطبيق Tigro للجوال - التوفر (Odoo 18) + التقدير

## 1) ملخص (متوفر / غير متوفر)

### متوفر في Odoo (بدون تخصيص)

- المصادقة (جلسات) + وصول عملاء البوابة (Portal).
- ملف العميل والعناوين (`res.partner` + عناوين فرعية).
- البلدان/المناطق، و(إن كانت مثبتة) المدن.
- كتالوج التجارة الإلكترونية: التصنيفات، المنتجات، المتغيرات، الصور، حقول SEO.
- المنتجات المرتبطة/الترقية (upsell)/البيع التقاطعي (cross-sell).
- السلة + تدفق الدفع (طلب بيع موقع محفوظ في الجلسة).
- الطلبات/الفواتير (Portal).
- إطار مزودي الدفع.
- إطار حساب رسوم الشحن (`delivery` + تكامل الدفع).
- إطار القسائم/العروض/الولاء (`loyalty`, `sale_loyalty`, `website_sale_loyalty`).
- قائمة الرغبات (`website_sale_wishlist`).
- تذاكر الدعم (بوابة العملاء) (`helpdesk`).
- إطار التقييمات (`rating` + المنتج يستخدم `rating.mixin`).
- صفحات CMS ومقاطع الأسئلة الشائعة (الموقع).

### متوفر عبر تخصيصات Tigro-Live الحالية

- تكامل دفع MyFatoorah (مزود دفع + مسارات JSON تحت `/payment/myfatoorah/...`).
- منطق إضافة تغليف الهدايا إلى السلة (مسار JSON: `/shop/cart/giftwrap`).
- العلامات التجارية: موديل مخصص `product.brand`.
- حقول إضافية:
  - `res.partner`: `block`, `floor`, `apartment`.
  - `product.template`: `tigro_reference_number`.
  - `sale.order`: `sales_channel`.

### غير متوفر (يتطلب تخصيص جديد بالإضافة إلى طبقة الـ API)

- مفهوم دخول الضيف (نقطة نهاية مخصصة لجلسة/حساب ضيف).
- تدفق Refresh Token (إذا كان مطلوبًا حسب عقد تطبيق الجوال).
- التحقق عبر OTP وإعادة الإرسال.
- موديل جغرافي لمناطق/Zones (إذا كان مطلوبًا حسب التطبيق).
- مفهوم Flash Sale.
- مناطق التوصيل، ETA، فترات/ساعات التوصيل، التتبع الحي (يتطلب تكامل لوجستي).
- تأكيد الاستلام وطلبات الإرجاع (تدفقات خدمة ذاتية للعميل).
- تدفق شحن/تعبئة المحفظة (Wallet Top-up).
- مستوى/شريحة الولاء + برنامج الإحالة (Referral).
- عقد/موديل إشعارات للجوال (أكثر من المراسلة القياسية).
- دور/تطبيق السائق + نقاط نهاية وتدفق عمل السائق للطلبات.
- نقاط نهاية تحليلات مجمعة (مبيعات/منتجات/حملات/سلة مهجورة) كعقد Mobile API.

## 2) إضافات Odoo جديدة سيتم تطويرها (طبقة API)

| الإضافة (جديدة) | النطاق (Endpoints) | أهم الاعتمادات | التقدير (ساعات) |
|---|---|---|---:|
| `tigro_mobile_api_core` | أساس الـ API، صيغة الاستجابة، معالجة الأخطاء، استراتيجية المصادقة/الجلسة/التوكن | `web`, `portal` | 24 |
| `tigro_mobile_api_customer` | الملف الشخصي، التفضيلات، العناوين، الموقع | `base`, `portal` | 20 |
| `tigro_mobile_api_catalog` | التصنيفات، العلامات، المنتجات، البحث، التوفر | `website_sale`, `stock` | 24 |
| `tigro_mobile_api_shop` | السلة، الدفع، الطلبات، الفواتير، طرق الدفع | `website_sale`, `sale`, `account`, `payment`, `delivery` | 40 |
| `tigro_mobile_api_loyalty` | القسائم، العروض، الولاء، نقاط نهاية المحفظة | `loyalty`, `sale_loyalty`, `website_sale_loyalty` | 32 |
| `tigro_mobile_api_support` | تذاكر الدعم + التعليقات | `helpdesk`, `mail`, `portal` | 16 |
| `tigro_mobile_api_content` | محتوى CMS (الرئيسية/البنرات/الصفحات/FAQ) + إعدادات/أعلام النظام | `website` | 16 |
| `tigro_mobile_api_engagement` | المراجعات + نقاط نهاية الإشعارات | `rating`, `mail` | 24 |
| `tigro_mobile_api_logistics` | مناطق/ETA/Slots/تتبع حي + تدفق عمل السائق | `delivery`, `stock_delivery` + تكامل خارجي | 80 |
| `tigro_mobile_api_analytics` | نقاط نهاية التحليلات (مبيعات/منتج/حملة/سلة) | `sale`, `website_sale` (+ وحدات تسويق إذا لزم) | 24 |

## 3) تقدير الجهد (بالساعات)

### المرحلة 1 (واجهة API للجوال + التجارة الإلكترونية)

| مسار العمل | التقدير (ساعات) | أيام @ 8 ساعات/يوم | أسابيع @ 5 أيام/أسبوع |
|---|---:|---:|---:|
| أساس الـ API + استراتيجية المصادقة | 24 | 3 | 0.6 |
| العميل/الملف/العناوين/الموقع | 20 | 2.5 | 0.5 |
| الكتالوج/المنتجات/العلامات/البحث/التوفر | 24 | 3 | 0.6 |
| السلة/الدفع/الطلبات/الفواتير | 40 | 5 | 1.0 |
| تكامل المدفوعات (بما في ذلك مواءمة MyFatoorah مع عقد تطبيق الجوال) | 24 | 3 | 0.6 |
| الولاء/العروض/المحفظة (حسب عقد CSV) | 32 | 4 | 0.8 |
| قائمة الرغبات | 8 | 1 | 0.2 |
| المراجعات | 12 | 1.5 | 0.3 |
| CMS + إعدادات/أعلام النظام | 16 | 2 | 0.4 |
| تذاكر الدعم (Helpdesk) | 16 | 2 | 0.4 |
| QA + دورات اختبار + تسليم/تسليم نهائي | 24 | 3 | 0.6 |
| **إجمالي المرحلة 1** | **216** | **27** | **5.4** |

### المرحلة 2 (اللوجستيات المتقدمة + السائق + التحليلات)

| مسار العمل | التقدير (ساعات) | أيام @ 8 ساعات/يوم | أسابيع @ 5 أيام/أسبوع |
|---|---:|---:|---:|
| مناطق/ETA/Slots/تتبع حي (يعتمد على التكامل) | 40 | 5 | 1.0 |
| نقاط نهاية تطبيق السائق + تدفق حالة الطلب للسائق | 40 | 5 | 1.0 |
| شريحة/مستوى الولاء + الإحالة (إذا كان مطلوبًا للتطبيق) | 16 | 2 | 0.4 |
| الإرجاعات + تدفقات تأكيد الاستلام | 16 | 2 | 0.4 |
| نقاط نهاية تحليلات API (تجميع + صلاحيات وصول) | 24 | 3 | 0.6 |
| QA + أداء + تعزيز الأمان | 16 | 2 | 0.4 |
| **إجمالي المرحلة 2** | **152** | **19** | **3.8** |

### الإجمالي الكلي (المرحلة 1 + المرحلة 2)

| الإجمالي | الساعات | الأيام @ 8 ساعات/يوم | الأسابيع @ 5 أيام/أسبوع |
|---|---:|---:|---:|
| **الإجمالي الكلي** | **368** | **46** | **9.2** |

### الافتراضات (لأجل التقدير)

- يتم تنفيذ نقاط REST كـ Odoo HTTP controllers تُرجع JSON.
- يتم فرض صلاحيات الوصول وقواعد السجلات (Record Rules) لكل نقطة نهاية.
- تكامل اللوجستيات/التتبع الحي الخارجي يُعامل كاعتماد منفصل يؤثر على المرحلة 2.

## 4) مصفوفة نقاط النهاية (Endpoint matrix)

| المجال | اسم API | الطريقة | المسار | Odoo قياسي (بدون تخصيص) | موجود في Tigro-Live (مخصص) | يحتاج API / طبقة واجهة جديدة | ملاحظات / دليل |
|---|---|---:|---|---|---|---|---|
| Auth | Login | POST | `/api/auth/login` | نعم | لا | نعم | Odoo يوفر تسجيل دخول JSON عبر `/web/session/authenticate` (`odoo/addons/web/controllers/session.py`). |
| Auth | Register | POST | `/api/auth/register` | نعم | لا | نعم | التسجيل (Signup) موجود عبر `auth_signup` (`odoo/addons/auth_signup`). |
| Auth | Guest Login | POST | `/api/auth/guest` | لا | لا | نعم | مفهوم *جلسة/حساب ضيف* غير متوفر كميزة API قياسية (الموقع لديه مستخدم عام، لكن ليس نقطة نهاية مخصصة لحساب ضيف). |
| Auth | Social Login | POST | `/api/auth/social` | نعم | لا | نعم | OAuth متوفر عبر `auth_oauth` (`odoo/addons/auth_oauth`). |
| Auth | Logout | POST | `/api/auth/logout` | نعم | لا | نعم | تسجيل الخروج للجلسة موجود (`/web/session/destroy`, `/web/session/logout`). |
| Auth | Refresh Token | POST | `/api/auth/refresh` | لا | لا | نعم | Odoo القياسي يعتمد على كوكي الجلسة؛ لا توجد نقطة REST قياسية لـ refresh-token ضمن الإضافات المفحوصة. |
| Auth | Forgot Password | POST | `/api/auth/forgot-password` | نعم | لا | نعم | إعادة تعيين كلمة المرور مدعومة عبر `auth_signup` (تدفق الويب القياسي). |
| Auth | Reset Password | POST | `/api/auth/reset-password` | نعم | لا | نعم | إعادة تعيين كلمة المرور مدعومة عبر `auth_signup` (تدفق الويب القياسي). |
| Auth | Change Password | POST | `/api/auth/change-password` | نعم | لا | نعم | تغيير كلمة المرور متاح عبر تدفقات البوابة/الحساب (لا يوجد مسار مطابق `/api/...`). |
| Auth | Deactivate Account | POST | `/api/auth/deactivate` | لا | لا | نعم | إيقاف الحساب ذاتيًا ليس ميزة قياسية للعميل؛ يتطلب منطقًا مخصصًا (أرشفة المستخدم/قواعد الشريك). |
| Auth | Delete Account | DELETE | `/api/auth/delete` | لا | لا | نعم | الحذف الذاتي ليس قياسيًا؛ يتطلب منطقًا مخصصًا وقواعد GDPR/الاحتفاظ. |
| Auth | Verify Email/OTP | POST | `/api/auth/verify` | جزئي | لا | نعم | التحقق عبر البريد موجود في تدفقات التسجيل؛ التحقق عبر OTP ليس قياسيًا. |
| Auth | Resend OTP | POST | `/api/auth/resend-otp` | لا | لا | نعم | إعادة إرسال OTP غير موجودة كميزة قياسية ضمن الإضافات المفحوصة. |
| Profile | Get Profile | GET | `/api/customer/profile` | نعم | لا | نعم | ملف العميل هو `res.partner` + حساب بوابة (`odoo/addons/portal`). |
| Profile | Update Profile | PUT | `/api/customer/profile` | نعم | لا | نعم | تحديث الشريك موجود عبر تدفقات تعديل حساب البوابة. |
| Profile | Get Preferences | GET | `/api/customer/preferences` | جزئي | لا | نعم | اللغة موجودة؛ تفضيلات إشعارات خاصة بالتطبيق ليست موجودة ككائن قياسي. |
| Profile | Update Preferences | PUT | `/api/customer/preferences` | جزئي | لا | نعم | كما سبق. |
| Address | Get Addresses | GET | `/api/address` | نعم | لا | نعم | Odoo يدعم عناوين متعددة للشريك (تسليم/فوترة/أخرى). |
| Address | Add Address | POST | `/api/address` | نعم | لا | نعم | إنشاء العنوان موجود (عناوين أبناء الشريك). |
| Address | Update Address | PUT | `/api/address/{id}` | نعم | لا | نعم | تحديث العنوان موجود. |
| Address | Delete Address | DELETE | `/api/address/{id}` | نعم | لا | نعم | حذف العنوان موجود (مع فرض قيود البوابة/قواعد الوصول). |
| Address | Set Default Address | POST | `/api/address/{id}/default` | نعم | لا | نعم | العنوان الافتراضي للشحن/الفوترة يُخزن على `sale.order`/الشريك؛ يتطلب منطق API. |
| Location | Get Countries | GET | `/api/location/countries` | نعم | لا | نعم | `res.country` قياسي. |
| Location | Get Cities | GET | `/api/location/cities` | نعم (إذا كانت مثبتة) | لا | نعم | موديل المدن موجود في `base_address_extended` (`odoo/addons/base_address_extended/models/res_city.py`). |
| Location | Get Provinces | GET | `/api/location/provinces` | نعم | لا | نعم | `res.country.state` قياسي. |
| Location | Get Areas | GET | `/api/location/areas` | لا | لا | نعم | المناطق/الـ zones ليست موديلًا جغرافيًا قياسيًا ضمن الإضافات المفحوصة. |
| Catalog | Get SubCategories | GET | `/api/categories/{id}` | نعم | لا | نعم | `product.public.category` (تصنيفات الموقع) قياسي في `website_sale`. |
| Catalog | Get Brands | GET | `/api/catalog/brands` | لا | نعم | نعم | `Tigro-Live` يوفر موديلًا مخصصًا `product.brand` (انظر `custom_18_part/Tigro-Live/emipro_theme_base/models/product_brand.py`). |
| Products | Search Products | GET | `/api/products/search` | نعم | لا | نعم | البحث موجود في `website_sale` ونماذج المنتج؛ يحتاج طبقة API. |
| Products | Get Product Listing | GET | `/api/products` | نعم | لا | نعم | قائمة المنتجات موجودة في `website_sale`؛ تحتاج طبقة API. |
| Products | Get Product Details | GET | `/api/products/{id}` | نعم | لا | نعم | تفاصيل المنتج موجودة؛ تحتاج طبقة API. |
| Products | Get Related Products | GET | `/api/products/{id}/related` | نعم | لا | نعم | المنتجات المرتبطة متاحة عبر `accessory_product_ids` / `alternative_product_ids` (انظر `odoo/addons/website_sale/models/product_template.py`). |
| Products | Get Upsell Products | GET | `/api/products/{id}/upsell` | نعم | لا | نعم | استراتيجية upsell/cross-sell مدعومة عبر alternative/accessory products (قياسي). |
| Products | Get Featured Products | GET | `/api/products/featured` | جزئي | لا | نعم | يمكن تنفيذها عبر نشر الموقع/الترتيب/الشرائط؛ لا يوجد API قياسي واحد لميزة “featured”. |
| Products | Get Flash Sale Products | GET | `/api/products/flash-sale` | لا | لا | نعم | مفهوم Flash-sale غير موجود كموديل/ميزة قياسية ضمن الإضافات المفحوصة. |
| Products | Check Availability | GET | `/api/products/{id}/availability` | نعم | لا | نعم | توفر المخزون موجود عبر stock + `website_sale_stock`. |
| Cart | Get Cart | GET | `/api/cart` | نعم | لا | نعم | السلة القياسية هي `sale.order` محفوظة في جلسة الموقع (`website_sale`). |
| Cart | Add Item | POST | `/api/cart/items` | نعم | لا | نعم | الموقع يوفر مسارات تحديث سلة مثل `/shop/cart/update_json` (`odoo/addons/website_sale`). |
| Cart | Update Item Qty | PUT | `/api/cart/items/{id}` | نعم | لا | نعم | كما سبق. |
| Cart | Remove Item | DELETE | `/api/cart/items/{id}` | نعم | لا | نعم | كما سبق. |
| Cart | Clear Cart | DELETE | `/api/cart` | نعم | جزئي | نعم | `Tigro-Live` لديه `/shop/clear_cart` (انظر `custom_18_part/Tigro-Live/emipro_theme_base/controllers/main.py`) لكن ليس `/api/cart`. |
| Cart | Apply Coupon | POST | `/api/cart/coupon` | نعم | لا | نعم | القسائم/الولاء مدعومة عبر `loyalty`, `sale_loyalty`, `website_sale_loyalty` (انظر manifests تحت `odoo/addons`). |
| Cart | Remove Coupon | DELETE | `/api/cart/coupon` | نعم | لا | نعم | كما سبق. |
| Cart | Apply Wallet | POST | `/api/cart/wallet` | جزئي | لا | نعم | eWallet جزء من `loyalty` (manifest يذكر eWallet) لكن تطبيقها في الدفع يحتاج طبقة تكامل API. |
| Cart | Remove Wallet | DELETE | `/api/cart/wallet` | جزئي | لا | نعم | كما سبق. |
| Cart | Check Stock | POST | `/api/cart/check-stock` | نعم | لا | نعم | فحوصات المخزون موجودة لكن نقطة API صريحة ليست قياسية. |
| Checkout | Validate Checkout | POST | `/api/checkout/validate` | نعم | لا | نعم | تدفق الدفع موجود في `website_sale`؛ نقطة API مخصصة. |
| Checkout | Get Delivery Fee | GET | `/api/checkout/delivery-fee` | نعم | لا | نعم | حساب رسوم الشحن موجود عبر `delivery` + تكامل دفع `website_sale`. |
| Checkout | Select Address | POST | `/api/checkout/address` | نعم | لا | نعم | اختيار العنوان موجود ضمن تدفق الدفع (الموقع). |
| Checkout | Select Payment Method | POST | `/api/checkout/payment-method` | نعم | لا | نعم | اختيار مزود الدفع موجود في الدفع القياسي (`payment` addons). |
| Checkout | Place Order | POST | `/api/checkout/place-order` | نعم | لا | نعم | إنشاء الطلب موجود؛ نقطة API مخصصة. |
| Checkout | Retry Payment | POST | `/api/checkout/retry-payment` | نعم | لا | نعم | إعادة محاولة الدفع ممكنة عبر معاملات الدفع القياسية؛ نقطة API مخصصة. |
| Orders | Get Orders List | GET | `/api/orders` | نعم | لا | نعم | طلبات البيع + وصول البوابة موجودان (`sale`, `portal`). |
| Orders | Get Order Details | GET | `/api/orders/{id}` | نعم | لا | نعم | كما سبق. |
| Orders | Cancel Order | POST | `/api/orders/{id}/cancel` | جزئي | لا | نعم | الإلغاء موجود، لكن قواعد الإلغاء للعميل غالبًا مخصصة. |
| Orders | Reorder | POST | `/api/orders/{id}/reorder` | نعم | لا | نعم | إعادة الطلب موجودة في أصول الموقع (`website_sale_reorder.js`). |
| Orders | Track Order | GET | `/api/orders/{id}/track` | جزئي | لا | نعم | حالة الطلب قياسية؛ التتبع يعتمد على تكامل شركة الشحن وعقد API مخصص. |
| Orders | Download Invoice | GET | `/api/orders/{id}/invoice` | نعم | لا | نعم | الفواتير قياسية (`account`) ويمكن عرضها في البوابة؛ نقطة API مخصصة. |
| Orders | Confirm Delivery | POST | `/api/orders/{id}/confirm` | لا | لا | نعم | تأكيد العميل للاستلام ليس ميزة قياسية. |
| Orders | Create Return | POST | `/api/orders/{id}/return` | لا | لا | نعم | طلبات الإرجاع من العميل ليست ميزة بوابة/API قياسية ضمن الإضافات المفحوصة. |
| Payments | Get Payment Methods | GET | `/api/payments/methods` | نعم | جزئي | نعم | مزودو الدفع موجودون في `payment`; ويشمل Tigro-Live مزود MyFatoorah (`custom_18_part/Tigro-Live/myfatoorah_gateway`). |
| Payments | Initiate Payment | POST | `/api/payments/initiate` | نعم | جزئي | نعم | MyFatoorah يعرض مسارات JSON تحت `/payment/myfatoorah/...` لكن ليس المسار المطلوب `/api/...`. |
| Payments | Verify Payment | POST | `/api/payments/verify` | نعم | جزئي | نعم | كما سبق. |
| Payments | Check Payment Status | GET | `/api/payments/{id}/status` | نعم | جزئي | نعم | معاملات الدفع القياسية موجودة؛ يحتاج مواءمة نقطة API. |
| Delivery | Get Delivery Zones | GET | `/api/delivery/zones` | لا | لا | نعم | شركات الشحن موجودة؛ موديل/نقطة “zones” ليست قياسية. |
| Delivery | Get Delivery ETA | GET | `/api/delivery/eta` | لا | لا | نعم | ETA يعتمد عادةً على تكامل لوجستي؛ غير موجود كميزة قياسية. |
| Delivery | Live Track Delivery | GET | `/api/delivery/{orderId}/live` | لا | لا | نعم | التتبع الحي ليس قياسيًا؛ يتطلب تكامل تتبع خارجي + API. |
| Delivery | Change Delivery Slot | POST | `/api/delivery/{orderId}/slot` | لا | لا | نعم | إدارة فترات التوصيل ليست قياسية ضمن الإضافات المفحوصة. |
| Wishlist | Get Wishlist | GET | `/api/wishlist` | نعم | لا | نعم | قائمة الرغبات موجودة عبر `website_sale_wishlist` (`odoo/addons/website_sale_wishlist`). |
| Wishlist | Add to Wishlist | POST | `/api/wishlist/items` | نعم | لا | نعم | كما سبق. |
| Wishlist | Remove from Wishlist | DELETE | `/api/wishlist/items/{id}` | نعم | لا | نعم | كما سبق. |
| Wallet | Get Wallet Balance | GET | `/api/wallet` | جزئي | لا | نعم | مفهوم eWallet موجود في `loyalty` (manifest يذكر eWallet). تحتاج API. |
| Wallet | Get Wallet Transactions | GET | `/api/wallet/transactions` | جزئي | لا | نعم | تاريخ الولاء موجود؛ نقطة API لمعاملات المحفظة مخصصة. |
| Wallet | Top-up Wallet | POST | `/api/wallet/topup` | لا | لا | نعم | شحن المحفظة يتطلب دفع + قواعد أعمال؛ غير موجود كميزة قياسية. |
| Loyalty | Get Loyalty Summary | GET | `/api/loyalty` | نعم | لا | نعم | برامج الولاء موجودة عبر `loyalty` (`odoo/addons/loyalty`). |
| Loyalty | Get Points History | GET | `/api/loyalty/transactions` | نعم | لا | نعم | تاريخ الولاء موجود (`loyalty_history`). |
| Loyalty | Redeem Points | POST | `/api/loyalty/redeem` | نعم | لا | نعم | الاستبدال موجود عبر قواعد/مكافآت الولاء؛ API مخصص. |
| Loyalty | Get Tier Info | GET | `/api/loyalty/tier` | لا | لا | نعم | ميزة الشريحة (Tier) غير موجودة في إضافة `loyalty` المفحوصة. |
| Loyalty | Get Referral Info | GET | `/api/loyalty/referral` | لا | لا | نعم | ميزة الإحالة غير موجودة في الإضافات المفحوصة. |
| Promotions | Get Available Promotions | GET | `/api/promotions` | نعم | لا | نعم | العروض/القسائم موجودة عبر `loyalty` + `website_sale_loyalty`. |
| Promotions | Validate Coupon | POST | `/api/promotions/validate-coupon` | نعم | لا | نعم | تحقق القسيمة موجود في منطق website sale loyalty؛ API مخصص. |
| Reviews | Get Product Reviews | GET | `/api/reviews/product/{id}` | نعم | لا | نعم | المنتج يدعم `rating.mixin` (`odoo/addons/website_sale/models/product_template.py`). |
| Reviews | Add Review | POST | `/api/reviews` | نعم | لا | نعم | يمكن إنشاء التقييمات؛ يحتاج controller API. |
| Reviews | Edit Review | PUT | `/api/reviews/{id}` | جزئي | لا | نعم | تعديل تقييمات المستخدم ليست عملية مكشوفة قياسيًا؛ قد تحتاج قواعد/Controller مخصص. |
| Reviews | Delete Review | DELETE | `/api/reviews/{id}` | جزئي | لا | نعم | كما سبق. |
| Reviews | Check Eligibility | GET | `/api/reviews/eligibility/{productId}` | لا | لا | نعم | قواعد الأهلية (مثل يجب أن يكون اشترى) مخصصة. |
| CMS | Get Home Content | GET | `/api/cms/home` | نعم | لا | نعم | صفحات/مقاطع الموقع موجودة؛ تجميع “الرئيسية” للجوال عبر API مخصص. |
| CMS | Get Banners | GET | `/api/cms/banners` | نعم | لا | نعم | يمكن إدارة البنرات عبر محتوى الموقع؛ نقطة API مخصصة. |
| CMS | Get Page by Slug | GET | `/api/cms/pages/{slug}` | نعم | لا | نعم | صفحات الموقع موجودة (`website`); نقطة API مخصصة. |
| CMS | Get FAQs | GET | `/api/cms/faqs` | نعم | لا | نعم | مقاطع FAQ موجودة (انظر `odoo/addons/website/static/src/snippets/s_faq_*`). نقطة API مخصصة. |
| System | Get App Config | GET | `/api/system/config` | جزئي | لا | نعم | معاملات الإعدادات موجودة (`ir.config_parameter`) لكن نقطة app-config مخصصة. |
| System | Maintenance mode | GET | `/api/system/maintenance` | لا | لا | نعم | نقطة maintenance-mode غير موجودة كميزة قياسية. |
| System | Get feature flags | GET | `/api/system/flags` | لا | لا | نعم | نقطة feature flags غير موجودة كميزة قياسية. |
| Notifications | Get Notifications | GET | `/api/notifications` | جزئي | لا | نعم | المراسلة موجودة (`mail`)، لكن “API إشعارات التطبيق” مخصصة. |
| Notifications | Mark as Read | POST | `/api/notifications/{id}/read` | جزئي | لا | نعم | يتطلب مواءمة لموديل مختار (mail.message / موديل إشعارات مخصص). |
| Notifications | Mark All as Read | POST | `/api/notifications/read-all` | جزئي | لا | نعم | كما سبق. |
| Notifications | Delete Notification | DELETE | `/api/notifications/{id}` | جزئي | لا | نعم | كما سبق. |
| Notifications | Get Unread Count | GET | `/api/notifications/unread-count` | جزئي | لا | نعم | كما سبق. |
| Gifts | Get Wrapping Options | GET | `/api/gifts/wrapping-options` | لا | نعم | نعم | تغليف الهدايا موجود في `Tigro-Live` (`odoo_website_giftwrap`)، لكنه يعرض فقط `/shop/cart/giftwrap`; عقد Mobile API مفقود. |
| Gifts | Get Card Designs | GET | `/api/gifts/card-designs` | لا | لا | نعم | لا يوجد دليل على ميزة “تصاميم بطاقات الهدايا” ضمن الموديلات المخصصة المفحوصة. |
| Gifts | Add Gift to Cart | POST | `/api/gifts/add-to-cart` | لا | جزئي | نعم | منطق إضافة التغليف موجود عبر `/shop/cart/giftwrap` (مخصص). يحتاج نقاط Mobile API مخصصة. |
| Gifts | Remove Gift from Cart | POST | `/api/gifts/remove-from-cart` | لا | لا | نعم | لم يتم العثور على نقطة إزالة في `odoo_website_giftwrap`; يتطلب تخصيص. |
| Support | Create Ticket | POST | `/api/support/tickets` | نعم | لا | نعم | Helpdesk موجود (`odoo/addons/helpdesk`) بما في ذلك قوالب البوابة؛ نقاط API مخصصة. |
| Support | Get My Tickets | GET | `/api/support/tickets` | نعم | لا | نعم | كما سبق. |
| Support | Get Ticket Details | GET | `/api/support/tickets/{id}` | نعم | لا | نعم | كما سبق. |
| Support | Add Ticket Comment | POST | `/api/support/tickets/{id}/comments` | نعم | لا | نعم | Chatter التذكرة موجود (`mail`)، لكن نقطة API مخصصة. |
| Analytics | Log Event | POST | `/api/analytics/events` | لا | لا | نعم | نقطة لتسجيل أحداث الواجهة (Frontend event logging) غير موجودة كميزة قياسية. |
| Driver | Driver login | POST | `/api/driver/login` | لا | لا | نعم | دور/تطبيق السائق ليس قياسيًا في Odoo. |
| Driver | Driver Logout | POST | `/api/driver/logout` | لا | لا | نعم | كما سبق. |
| Driver | Get Profile | GET | `/api/driver/profile` | لا | لا | نعم | كما سبق. |
| Driver | Update Driver | PUT | `/api/driver/profile` | لا | لا | نعم | كما سبق. |
| Driver | Pickup Order | PUT | `/api/driver/order/status` | لا | لا | نعم | يتطلب تدفق توصيل مخصص + موديلات تعيين السائق. |
| Analytics | Sales data | GET | `/api/analytics/sales` | نعم | لا | نعم | التقارير موجودة في Odoo، لكن نقطة تحليلات مجمعة مخصصة. |
| Analytics | Product performance | GET | `/api/analytics/product` | نعم | لا | نعم | التقارير موجودة؛ نقطة API مخصصة. |
| Analytics | Campaign performance | GET | `/api/analytics/campaign` | جزئي | لا | نعم | يعتمد على وحدات التسويق؛ لا يوجد نقطة مخصصة واضحة. |
| Analytics | Abandoned cart | GET | `/api/analytics/cart` | نعم | لا | نعم | سلوك السلة المهجورة موجود في `website_sale` (الاختبارات تشير له)، لكن نقطة API مخصصة. |

