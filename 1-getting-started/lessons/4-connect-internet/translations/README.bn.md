# আইওটি ডিভাইসকে ইন্টারনেটে সংযুক্তিকরণ

![A sketchnote overview of this lesson](../../../../sketchnotes/lesson-4.jpg)

> [Nitya Narasimhan](https://github.com/nitya) তৈরী করছেন এই স্কেচনোটটি। এটির বড় সংস্করণ দেখতে হলে ছবিটির উপর ক্লিক করতে হবে।

## লেকচার পূর্ববর্তী কুইজ

[লেকচার পূর্ববর্তী কুইজ](https://brave-island-0b7c7f50f.azurestaticapps.net/quiz/7)

## সূচনা

IoT শব্দে **I** হলো Internet - আইওটিতে "ইন্টারনেট" বলতে ক্লাউড এর মাধ্যমে সংযোগ এবং সেবা প্রদানের মাধ্যমে আইওটি যন্ত্রের বৈশিষ্ট্যসমূহ চালু করা, যন্ত্রের সাথে সংযুক্ত সেন্সর এর মাধ্যমে পরিমাপসমূহ সংগ্রহ করা এবং বার্তা প্রেরণের মাধ্যমে একচুয়েটরসমূহকে নিয়ন্ত্রণ করাকে বুঝায়। আইওটি যন্ত্রগুলি সাধারণত একটি স্ট্যান্ডার্ড কমিউনিকেশন প্রোটোকল ব্যবহার করে একটিমাত্র ক্লাউড আইওটি সেবাতে সংযুক্ত হয় এবং এই সেবাটি সব আইওটি এপ্লিকেশন এর সাথে সংযুক্ত থাকে যা কিনা যন্ত্রসমূহ থেকে প্রাপ্ত ডেটার মাধ্যমে এআই (কৃত্রিম বুদ্ধিমত্তা) সেবার সাহায্যে গুরুত্বপূর্ণ সিদ্ধান্ত নেওয়া থেকে শুরু করে ওয়েব এপস এর মাধ্যমে নিয়ন্ত্রণ করা বা প্রতিবেদনও তৈরি করে দিতে পারে।

> 🎓 সেন্সর এর মাধ্যমে তথ্য সংগ্রহ এবং সেই তথ্য ক্লাউডে প্রেরণ করাকে টেলিমেট্রি(Telemetry) বলে।

আইওটি যন্ত্রসমূহ ক্লাউড থেকে বার্তা গ্রহণ করতে সক্ষম। তবে এই বার্তাগুলোতে আদেশ থাকে - সেটি হল অভ্যন্তরীণভাবে কোনও কাজ সম্পন্ন করার নির্দেশনাবলী (যেমন রিবুট বা ফার্মওয়্যার আপডেট করা), অথবা একচুয়েটরকে ব্যবহার করা (যেমন একটি লাইট জ্বালানো)।

এই পাঠটিতে আইওটি যন্ত্রসমূহ কমিউনিকেশন প্রোটোকল ব্যবহার করে ক্লাউডে সংযুক্ত হওয়া এবং কি ধরনের তথ্য ক্লাউডে গ্রহণ বা প্রেরণ করে তা শিখবো। নিয়ন্ত্রিত ইন্টারনেট  লাইটে সংযুক্ত করা এবং এলইডি নিয়ন্ত্রনের লজিক কোডটিকে চলমান 'সার্ভার'-এ নেওয়া, এই দুটি কাজ হাতে-কলমে শিখবো।

এ পাঠ হতে যা যা শিখবোঃ

* [কমিউনিকেশন প্রটোকলসমূহ](#কমিউনিকেশন-প্রটোকলসমূহ)
* [এমকিউটিটি (Message Queueing Telemetry Transport-MQTT)](#Message-Queueing-Telemetry-Transport-MQTT)
* [টেলিমেট্রি](#টেলিমেট্রি)
* [কমান্ডসমূহ](#কমান্ডসমূহ)


## কমিউনিকেশন প্রটোকলসমূহ

আইওটি যন্ত্রসমূহ কয়েকটি জনপ্রিয় কমিউনিকেশন প্রটোকল ব্যবহার করে ইন্টারনেটের সাথে সংযুক্ত হয়। সবচেয়ে জনপ্রিয় হচ্ছে কোন ধরনের সার্ভার বা Broker এর মাধ্যমে বার্তা প্রচার/সাবস্ক্রাইব করা। আইওটি যন্ত্রসমূহ এই ব্রোকারের সাথে সংযুক্ত হয়ে টেলিমেট্রি প্রচার করে এবং কমান্ডগুলোতে সাবস্ক্রাইব করে। ক্লাউড সেবাগুলোও এই ব্রোকারের সাথে সংযুক্ত হয়ে সকল টেলিমেট্রি বার্তাগুলোতে সাবস্ক্রাইব করে এবং কমান্ডগুলোকে হয় একটি নির্দিষ্ট ডিভাইসে না হয় অনেকগুলো ডিভাইসের একটি গ্রুপে প্রেরণ করে।

![আইওটি যন্ত্রসমূহ এই মধ্যস্থতাকারীর (Broker) সাথে সংযুক্ত হয়ে টেলিমেট্রি প্রচার করে এবং আদেশগুলোতে সাবস্ক্রাইব করে। ক্লাউড সেবাগুলোও এই মধ্যস্থতাকারীর সাথে সংযুক্ত হয়ে সকল টেলিমেট্রি বার্তাগুলোতে সাবস্ক্রাইব করে এবং কমান্ডগুলোকে একটি নির্দিষ্ট ডিভাইসে প্রচার করে।](../../../../images/pub-sub.png)

আইওটি ডিভাইসগুলোর জন্য MQTT হলো সবচেয়ে জনপ্রিয় কমিউনিকেশন প্রটোকল যা এই পাঠে অন্তর্ভুক্ত করা হয়েছে। অন্যান্য প্রটোকলের মধ্যে AMQP এবং HTTP/HTTPS অন্তর্ভুক্ত আছে।

## Message Queueing Telemetry Transport-MQTT

[MQTT](http://mqtt.org) হল লাইটওয়েট, ওপেন স্ট্যান্ডার্ড মেসেজিং প্রোটোকল যা ডিভাইসগুলোর মধ্যে বার্তা প্রেরণ করতে পারে। ১৫ বছর পরে ওপেন স্ট্যান্ডার্ড হিসাবে এটি আইবিএম দ্বারা প্রকাশিত হয় যা পূর্বে তেলের পাইপলাইনগুলি পর্যবেক্ষণ করার জন্য ১৯৯৯ সালে নকশা করা হয়েছিল ।

MQTT এর একটি ব্রোকার এবং একাধিক ক্লায়েন্ট রয়েছে। সমস্ত ক্লায়েন্ট ব্রোকারের সাথে সংযুক্ত হয় এবং ব্রোকার সংশ্লিষ্ট ক্লায়েন্টদের বার্তা প্রেরণ করে। বার্তাগুলো কোনো বিশেষ গ্রাহককে সরাসরি প্রেরণ না করে বরং নামকরণ করা টপিকগুলি ব্যবহার করে একটি নির্দিষ্ট পথে পাঠানো হয়। একটি ক্লায়েন্ট একটি টপিক প্রচার করতে পারে এবং যেকোনো ক্লায়েন্ট যে ঐ টপিকে সাবস্ক্রাইব করে তা সে সম্পর্কিত বার্তা গ্রহণ করে।

![IoT device publishing telemetry on the /telemetry topic, and the cloud service subscribing to that topic](../../../../images/mqtt.png)

✅ কিছু গবেষণা করি। যদি অনেকগুলো আইওটি ডিভাইস থাকে তাহলে কিভাবে নিশ্চিত হবো যে  MQTT- ব্রোকার সবগুলো বার্তা নিয়ন্ত্রন করতে পারবে কিনা?

### আইওটি ডিভাইসটি MQTT-তে সংযুক্তিকরণ

ইন্টারনেটের মাধ্যমে নাইটলাইটকে নিয়ন্ত্রণ করার প্রথম ধাপ হচ্ছে সেটিকে MQTT- ব্রোকার এর সাথে সংযুক্ত করা।

#### কাজ

আইওটি ডিভাইসটি MQTT-ব্রোকার এ সংযুক্ত করা।

পাঠের এই অংশটিতে আইওটি নাইটলাইটিকে ইন্টারনেটে সংযুক্ত করি যাতে করে সেটিকে দূর থেকে নিয়ন্ত্রণ করা যায়। এই পরবর্তী পাঠে, আইওটি ডিভাইসটি  MQTT-র মাধ্যমে একটি টেলিমেট্রি বার্তা লাইটের লেভেলসহ(সেন্সর এর ভ্যালু) পাবলিক MQTT ব্রোকারে পাঠাবে এবং  সেটি কতিপয় সার্ভার দ্বারা নেওয়া হবে যেটাতে কোডটি লেখা হয়েছিলো। এই কোডটি লাইটের লেভেল/সেন্সর ভ্যালু যাচাই করবে এবং যাচাই করার পর একটি আদেশমূলক বার্তা আইওটি লাইটটিতে/ডিভাইসে পাঠাবে যাতে আদেশ হিসেবে বলা থাকবে যে এলইডিটি অন না অফ হবে।

বাস্তবিক ক্ষেত্রে এমন অবস্থা হতে পারে যেখানে অনেক লাইট সেন্সর রয়েছে (যেমন স্ট্যাডিয়াম) এবং সেই লাইটগুলো অন করার সিদ্ধান্ত নেওয়ার পূর্বে ওই একাধিক লাইট সেন্সর এর তথ্য সংগ্রহ করার প্রয়োজন হতে পারে।  শুধুমাত্র একটি সেন্সর মেঘ বা পাখি দ্বারা আবৃত থাকে লাইটগুলি অন হওয়া থেকে বন্ধ রাখতে পারে, যদিও অন্য সেন্সরগুলি পর্যাপ্ত আলো শনাক্ত করেও।

✅ কমান্ড প্রেরণের আগে একাধিক সেন্সর এর তথ্য মূল্যায়নের জন্য অন্য কি পরিস্থিতিগুলো বিবেচিত হতে পারে?

এসাইনমেন্টের অংশ হিসেবে MQTT ব্রোকার সেটাপের এই  জটিলতার সাথে মোকাবেলা করার চেয়ে বরং চাইলে সেটাপটি পাবলিক টেস্ট সার্ভার ব্যবহার করা যবে যেটি [Eclipse Mosquitto](https://www.mosquitto.org)(একটি ওপেন সোর্স MQTT ব্রোকার)-এ  রান হবে। এই টেস্ট ব্রোকারটি [test.mosquitto.org](https://test.mosquitto.org)-এ পাওয়া যাবে যা জনসাধারনের জন্য উন্মুক্ত। MQTT ক্লায়েন্ট এবং সার্ভার এর জন্য এটি একটি অসাধারণ টুল কারণ এটিতে সেটাপ করতে কোনো একাউন্টের প্রয়োজন নেই।

> 💁 এই টেস্ট ব্রোকারটি উন্মুক্ত যা মোটেই সুরক্ষিত নয়। যে কেউ বুঝতে পারবে এতে কি পাবলিশ করা হয়েছে, তাই যে তথ্যগুলোতে গোপনীয় রাখা জরুরি সেগুলো এতে ব্যবহার না করার পরামর্শ রইল।

![A flow chart of the assignment showing light levels being read and checked, and the LED begin controlled](../../../../images/assignment-1-internet-flow.png)

ডিভাইসটি MQTT ব্রোকারে সংযুক্ত করতে সংশ্লিষ্ট ধাপগুলো অনুসরণ করিঃ

* [আরডুইনো Wio টার্মিনাল](wio-terminal-mqtt.bn.md)
* [সিংগেল বোর্ড কম্পিউটার - রাস্পবেরি পাই/ভার্চুয়াল আইওটি ডিভাইস](single-board-computer-mqtt.bn.md)

### MQTT এর আরো গভীরে

টপিকগুলোতে শ্রেণিবিন্যাস থাকতে পারে যাতে ক্লায়েন্টরা ওয়াইল্ডকার্ড ব্যবহার করে এই শ্রেণিবিন্যাসের বিভিন্ন স্তরে সাবস্ক্রাইব করতে পারে। যেমন, তাপমাত্রার টেলিমেট্রি বার্তাগুলো  `/telemetry/temperature` এই টপিকে এবং আর্দ্রতার বার্তাগুলো `/telemetry/humidity` এই টপিকে পাঠানো, তারপর ক্লাউড এপটি `/telemetry/*` এই টপিকে সাবস্ক্রাইব করে তাপমাত্রা এবং আর্দ্রতা উভয়ের টেলিমেট্রি বার্তাগুলো গ্রহণ করবে।

বার্তা গ্রহণের নিশ্চয়তা প্রদান করতে বার্তাগুলো কোয়ালিটি অফ সার্ভিস(QoS) এর সাথে পাঠানো হয়।

* সর্বাধিক একবার – বার্তা শুধুমাত্র একবারই পাঠানো হয় এবং ক্লায়েন্ট আর ব্রোকার বার্তাটি ডেলিভারীর প্রাপ্তি স্বীকার করতে কোনো অতিরিক্ত পদক্ষেপ নেয় না (fire and forget)।
* অন্তত একবার – স্বীকারোক্তি গৃহীত না হওয়া পর্যন্ত বার্তা প্রেরক একাধিকবার বার্তা প্রেরণের চেষ্টা করেছিল (acknowledged delivery)।
* ঠিক একবার – শুধু একটি বার্তা গৃহীত হয়েছে তা নিশ্চিত করতে প্রাপক এবং প্রেরক একটি দ্বি-স্তরের যোগাযোগ করে যাকে হ্যান্ডশেক এর সাথে তুলনা করা যায় (assured delivery)।

✅ কোন পরিস্থিতিতে fire and forget বার্তার পরেও একটি assured delivery এর বার্তা প্রয়োজন হতে পারে?

যদিও নামটি মেসেজ কিউয়িং (ইংরেজি প্রথম অক্ষরগুলো নিয়ে MQTT), এটি আসলে বার্তার সারিকে বুঝায় না। এর অর্থ হল যদি কোন ক্লায়েন্ট সংযোগ বিচ্ছিন্ন করে পুনরায় সংযোগ স্থাপন করার সময় QoS প্রসেস ব্যবহার করে ইতোমধ্যে প্রসেসকৃত বার্তাগুলো বাদে বিচ্ছিন্ন অবস্থায় প্রেরিত বার্তাগুলো গৃহীত হবে না। ওই বার্তাগুলোকে মনে রাখাতে একটি ফ্ল্যাগ সেট করা হয়।  যদি এই ফ্ল্যাগ সেট করা থাকে তবে MQTT-ব্রোকার একটি টপিকে প্রেরিত সর্বশেষ বার্তাটি ওই ফ্ল্যাগসহিত জমা রাখবে এবং পরবর্তীতে কোনো ক্লায়েন্ট যদি এই টপিকে সাবস্ক্রাইব করে তাকে প্রেরণ করা হয়। এই পদ্ধতিতে ক্লায়েন্টগুলো সবর্দা সর্বশেষ বার্তাগুলো পেয়ে থাকে।

MQTT একটি এলাইভ(alive) ফাংশনকে সমর্থন করে এবং এই ফাংশনটি বার্তাগুলোর মধ্যে দীর্ঘ বিরতির সময় সংযোগটি চালু আছে কিনা তা পরীক্ষা করে।

> 🦟 [Eclipse Foundation এর Mosquitto-তে](https://mosquitto.org) একটি ফ্রী MQTT ব্রোকার রয়েছে যাতে MQTT-সম্পর্কিত পরীক্ষা করা যাবে, MQTT-ব্রোকার এর সাথে কোড টেস্ট করা যাবে যা এই [test.mosquitto.org](https://test.mosquitto.org)  হোস্ট করা থাকে।

MQTT- সংযোগসমূহ পাবলিক ও উন্মুক্ত থাকতে পারে অথবা ইউজারনেইম, পাসওয়ার্ড এবং সার্টিফিকেট ব্যবহারের মাধ্যমে এনক্রিপটেড ও সুরক্ষিত থাকতে পারে।

> 💁 MQTT TCP/IP এর মাধ্যমে যোগাযোগ করে, এটি HTTP এর মতোই একটি নেটওয়ার্ক প্রোটকল তবে একটি ভিন্ন পোর্টবেইজড। ব্রাউজারে চলমান ওয়েব অ্যাপ্লিকেশনগুলির সাথে যোগাযোগের জন্য ওয়েবসকেটের পরিবর্তে বা ফায়ারওয়ালগুলি বা অন্যান্য নেটওয়ার্কিং রুলস যা স্ট্যান্ডার্ড সংযোগগুলিতে MQTT ব্লক করে এমন পরিস্থিতিতে MQTT ব্যবহার করা যাবে।

## টেলিমেট্রি

টেলিমেট্রি শব্দটি গ্রীক থেকে উদ্ভূত হয়েছিলো যার অর্থ দূরবর্তী থেকে পরিমাপ করা। টেলিমেট্রি হল সেন্সর থেকে ডেটা সংগ্রহ এবং সেই ডেটা ক্লাউডে প্রেরণ করা ।

> 💁 প্রাচীনতম টেলিমেট্রি ডিভাইসগুলি ফ্রান্সে ১৮৭৪ সালে উদ্ভাবিত হয়েছিল এবং Mont Blanc থেকে প্যারিসে রিয়েল-টাইম আবহাওয়া এবং তুষারের গভীরতা প্রেরণ করেছিল। এটি বাস্তবিক তারগুলি ব্যবহার করত কারণ ওয়্যারলেস প্রযুক্তিগুলি তখন ছিল  না।

পাঠ 1 থেকে স্মার্ট থার্মোস্টেটের উদাহরণটি দেখি।

![একাধিক রুম সেন্সর ব্যবহার করে ইন্টারনেটে সংযুক্ত একটি থার্মোস্ট্যাট](../../../../images/telemetry.png)

টেলিমেট্রি সংগ্রহের জন্য থার্মোস্ট্যাট এর তাপমাত্রাভিত্তিক সেন্সর রয়েছে। এটিতে একটি তাপমাত্রাভিত্তিক সেন্সর বিল্টইন থাকে এবং ওয়ারলেস  প্রোটোকল যেমন [ব্লুটুথ লো এনার্জি](https://wikipedia.org/wiki/Bluetooth_Low_Energy) (BLE) এর মাধ্যমে একাধিক তাপমাত্রাভিত্তিক সেন্সর এর সাথে  সংযুক্ত হতে পারে।

উদাহরণস্বরূপ একটি টেলিমেট্রি ডেটা যা প্রেরণ করা হবে, তা হলো

| নাম | মান | বর্ণনা |
| ---- | ----- | ----------- |
| `thermostat_temperature` | 18°C | থার্মোস্ট্যাট এর বিল্ট-ইন তাপামাত্রাভিত্তিক সেন্সর দ্বারা তাপমাত্রা পরিমাপ করা। |
| `livingroom_temperature` | 19°C |  দূরবর্তী তাপমাত্রাভিত্তিক সেন্সর(remote temperature sensor) দ্বারা তাপমাত্রা পরিমাপ করা হয় যেটিকে `livingroom` নামকরণ করা হয়েছে যাতে এটি যেই রুমে আছে সেই রুমকে শনাক্ত করতে পারে। |
| `bedroom_temperature` | 21°C | দূরবর্তী তাপমাত্রাভিত্তিক সেন্সর(remote temperature sensor) দ্বারা তাপমাত্রা  পরিমাপ করা হয় যেটিকে ` bedroom ` নামকরণ করা হয়েছে যাতে এটি যেই রুমে আছে সেই রুমকে শনাক্ত করতে পারে। |

ক্লাউড সেবা এই টেলিমেট্রি ডেটা ব্যবহার করে তাপকে নিয়ন্ত্রণ করতে কী আদেশ পাঠাবে তার সিদ্ধান্ত নিতে পারে।

### টেলিমেট্রি আইওটি ডিভাইসে প্রেরণ

নাইটলাইটটিকে ইন্টারনেটের মাধ্যমে নিয়ন্ত্রণ করার পরবর্তী অংশটি হলো লাইট লেভেল টেলিমেট্রি MQTT- ব্রোকারের টেলিমেট্রি টপিকে পাঠানো।

#### কাজ

লাইট লেভেল টেলিমেট্রি MQTT- ব্রোকারে পাঠানো।

ডেটা JSON হিসাবে এনকোড করে পাঠানো হয় – JSON হলো JavaScript Object Notation এর সংক্ষিপ্ত রূপ, যা কী/ভ্যালু পেয়ার ব্যবহার করে ডেটাকে এনকোডেড টেক্সট এ রূপান্তরের জন্য একটি স্ট্যান্ডার্ড ।

✅ JSON সম্পর্কে জ়েনে না থাকলে তা এই [JSON.org documentation](https://www.json.org/) থেকে শিখতে পারবো।

ডিভাইস থেকে MQTT-ব্রোকারের কাছে টেলিমেট্রি প্রেরণের জন্য নীচের পদক্ষেপটি অনুসরণ করিঃ

* [আরডুইনো Wio টার্মিনাল](wio-terminal-telemetry.bn.md)
* [সিংগেল বোর্ড কম্পিউটার - রাস্পবেরি পাই/ভার্চুয়াল আইওটি ডিভাইস](single-board-computer-telemetry.bn.md)

### MQTT ব্রোকার হতে টেলিমেট্রি গ্রহণ

টেলিমেট্রি পাঠানোর কোন অর্থ নেই যদি  অন্য প্রান্তে এটিকে গ্রহণ করার মতো কিছু না থাকে। লাইট লেভেল টেলিমেট্রিটির ডেটা প্রক্রিয়া করার জন্য এটিকে কিছু গ্রাহকের এর প্রয়োজ়নীয়তা আছে। এই 'সার্ভার' কোডটি সেই ধরণের কোড যা বৃহত্তর আইওটি অ্যাপ্লিকেশনের অংশ হিসাবে একটি ক্লাউড সেবাতে স্থাপন করবো, তবে এখানে স্থানীয়ভাবে এই কোডটি লোকাল কম্পিউটারে চালাতে পারবো (বা আমাদের Pi-তে সরাসরি কোডিং করতে পারবো)। সার্ভার কোডে একটি পাইথন অ্যাপ থাকে যা MQTT-র মাধ্যমে লাইটে লেভেলগুলোর টেলিমেট্রি বার্তা গ্রহণ করতে পারে। এই পাঠের পরবর্তীতে, এটিতে কমান্ডের মেসেজ রিপ্লেতে পাঠাবো যাতে নির্দেশনা থাকবে যে এলইডি অন না অফ হবে।

✅ কিছু গবেষনা করিঃ MQTT বার্তাগুলোর কী হবে যদি কোনো গ্রাহক না থাকে?

#### পাইথন এবং ভিএস কোড ইন্সটল করি

লোকালি Python এবং VS Code ইনস্টল করা না থাকলে সার্ভারে কোড দেওয়ার জন্য দুইটিকেই ইনস্টল করবো। যদি ভার্চুয়াল ডিভাইস ব্যবহার করি বা রাস্পবেরি পাইতে কাজ করি তবে এই পদক্ষেপটি এড়িয়ে যেতে পারি ।

##### কাজ

Python এবং VS Code ইন্সটল করি।

1. পাইথন ইন্সটল করি। [Python downloads page](https://www.python.org/downloads/) থেকে পাইথনে সর্বশেষ ভার্সনটি নির্দেশনা মোতাবেক ইনস্টল করি।

1. Visual Studio Code (VS Code) ইন্সটল করি। আমরা এই ইডিটরটি আমাদের ভার্চুয়াল ডিভাইসে পাইথনে কোড করার জন্য ব্যবহার করবো। [VS Code documentation](https://code.visualstudio.com?WT.mc_id=academic-17441-jabenn) থেকে Visual Studio Code (VS Code) নির্দেশনাগুলো অনসুরণ করে Visual Studio Code (VS Code) ইন্সটল করি।

    > 💁 এই পাঠের নির্দেশনাগুলো VS Code এর উপর ভিত্তি করে লেখা হলেও এই পাঠের জন্য আমরা আমাদের সুবিধামত টুল অর্থাৎ পাইথনবেইজ়ড যেকেনো আইডি বা ইডিটর ব্যবহার করতে পারি।

1. VS Code এর Pylance এক্সটেনশনটি ইন্সটল করি। পাইথন লেঙ্গুয়েজের সাপোর্টের জন্য এটি VS Code এর একটি এক্সটেনশন। এটি এক্সটেনশনটি VS Code-এ কিভাবে ইন্সটল করতে হয়  [Pylance extension documentation](https://marketplace.visualstudio.com/items?WT.mc_id=academic-17441-jabenn&itemName=ms-python.vscode-pylance) থেকে দেখে নেই।

#### পাইথনের ভার্চুয়াল এনভায়রনমেন্ট কনফিগারেশন

পাইথনের অন্যতম শক্তিশালী ফিচারটি হল [পিপ প্যাকেজ](https://pypi.org) ইনস্টল করার ক্ষমতা – এই কোডের প্যাকেজগুলি অন্যদের দ্বারা লেখা হয় যা পরবর্তীতে ইন্টারনেটে প্রকাশ করা হয়। কম্পিউটারে একটি কমান্ডের এর মাধ্যমে পিপ প্যাকেজ ইন্সটল করা যায়, তারপরে নির্দিষ্ট কোডটিতে সেই প্যাকেজটি ব্যবহার করতে পারবো। MQTT- এর মাধ্যমে যোগাযোগ করতে আমরা পিপ ব্যবহার করে প্যাকেজ ইন্সটল করব।

আমরা যখন কোনো প্যাকেজ ইন্সটল করি তখন তা পুরো কম্পিউটার জুড়ে থাকে এবং তা থেকে প্যাকেজ়ের ভার্সনজনিত সমস্যা দেখা দিতে পারে – যেমন একটি অ্যাপ্লিকেশন যখন  প্যাকেজ়ের একটি ভার্সনের উপর ভিত্তি করে চলে কিন্তু ভিন্ন এপ্লিকেশনের জন্য ওই একি প্যাকেজের নতুন ভার্সন ইন্সটল করলে আগের ভার্সনে ব্যাঘাত ঘটতে পারে। এই সমস্যা হতে উত্তরণের জন্য আমরা [Python virtual environment](https://docs.python.org/3/library/venv.html) ব্যবহার করবো এবং তাতে পাইথনের জন্য একটি ডেডিকেটেড ফোল্ডার থাকবে যাতে যখন আমরা আমাদের প্রয়োজনীয় পিপ প্যাকেজসমূহ ইন্সটল করলে তা এই ফোল্ডারে ইন্সটল হবে। 

##### কাজ

পাইথনের ভার্চুয়াল এনভায়রনমেন্ট কনফিগারেশন এবং MQTT পিপ প্যাকেজ ইন্সটল।

1. টার্মিনাল বা কমান্ড লাইন হতে আমাদের পছন্দসই লোকেশনে ডিরেক্টরি তৈরি এবং নেবিগেইট করতে নিচের কমান্ডগুলো রান দিইঃ

    ```sh
    mkdir nightlight-server
    cd nightlight-server
    ```

1. ভার্চুয়াল এনভায়রনমেন্ট `.venv` ফোল্ডারে বানানোর জন্য নিম্নের কমান্ডটি রান করি।

    ```sh
    python3 -m venv .venv
    ```

    > 💁 যদি Python 2 এর সাথে Python 3(লেটেস্ট ভার্সন) ইন্সটল করা থাকে তবে ভার্চুয়াল এনভায়রনমেন্ট বানানোর জন্য সঠিকভাবে `python3`-কে কল করতে হবে। যদি Python 2 ইন্সটল করা থাকে তবে `python` কল করলে তা  Python 3 এর পরিবর্তে Python 2-কে কল করবে।

1. ভার্চুয়াল এনভায়রনমেন্ট চালু করতে নিম্নের কমান্ড দিইঃ

    * Windows-এ রান করতে নিম্নের কমান্ডটি দিইঃ

        ```cmd
        .venv\Scripts\activate.bat
        ```

    * macOS বা Linux এ রান করতে নিম্নের কমান্ডটি দিইঃ

        ```cmd
        source ./.venv/bin/activate
        ```

1. ভার্চুয়াল এনভারনমেন্টটি একবার চালু হওয়ার পর ভার্চুয়াল এনভারনমেন্ট বানাতে পাইথনের যে ভার্সনটি ব্যবহৃত হয়েছিলো তা ডিফল্টভাবে  `python` কমান্ডটি সেই ভার্সনটিকে রান করাবে। পাইথনে ভার্সন জানতে নিম্নের কমান্ডটি রান করবোঃ

    ```sh
    python --version
    ```

    আউটপুটটি নিম্নের মতো হবেঃ

    ```output
    (.venv) ➜  nightlight-server python --version
    Python 3.9.1
    ```

    > 💁 পাইথনের ভার্সন ভিন্ন হতে পারে তবে ভার্সন ৩.৬ বা তারও বেশি হলে ভালো। যদি এই ভার্সন ইন্সটল না থাকে তবে এই ফোল্ডারটি ডিলিট করে পাইথনের নতুন ভার্সনটি ইন্সটল করে আবার চেষ্টা করি।

1. [Paho-MQTT](https://pypi.org/project/paho-mqtt/)(একটি জনপ্রিয় MQTT লাইব্রেরি) পিপ প্যাকেজটি ইন্সটল করতে নিম্নের কমান্ডটি রান করি।

    ```sh
    pip install paho-mqtt
    ```

    এই পিপ প্যাকেজটি শুধুমাত্র ভার্চুয়াল এনভায়রনমেন্টে ইন্সটল হবে।

#### সার্ভার কোডটি লিখি

এখন সার্ভার কোডটি পাইথনে লিখবো।

##### কাজ

সার্ভার কোডটি লিখি।

1. ভার্চুয়াল এনভায়রনমেন্টের ভিতরে `app.py` নামে একটি পাইথন ফাইল বানাতে টার্মিনাল বা কমান্ড লাইন হতে নিম্নের কমান্ডটি দিইঃ

    * Windows-এ রান করতে নিম্নের কমান্ডটি দিইঃ

        ```cmd
        type nul > app.py
        ```

    * macOS বা Linux এ রান করতে নিম্নের কমান্ডটি দিইঃ

        ```cmd
        touch app.py
        ```

1. কারেন্ট ফোল্ডারটি VS Code-এ ওপেন করিঃ

    ```sh
    code .
    ```

1. যখন VS Code-টি ওপেন হবে তখন তা পাইথনের ভার্চুয়াল এনভায়রনমেন্টটিকে চালু করবে। এটি উপরে স্ট্যাটাস বারে দেখা যাবেঃ

    ![VS Code showing the selected virtual environment](../../../../images/vscode-virtual-env.png)

1. যদি VS Code স্টার্ট হওয়ার সময়  VS Code টার্মিনালটি চালুরত অবস্থায় থাকে তবে ভার্চুয়াল এনভায়রনমেন্ট VS Code-এ একটিভেট হবে না। এর থেকে উত্তরণের সহজ উপায় হচ্ছে  **Kill the active terminal instance** বাটনটিতে ক্লিক করে টার্মিনালটিকে বন্ধ করে দিবো।

    ![VS Code Kill the active terminal instance button](../../../../images/vscode-kill-terminal.png)

1. নতুন VS Code টার্মিনাল চালু করতে *Terminal -> New Terminal – এ সিলেক্ট করবো বা `` CTRL+` `` প্রেস করবো। এই নতুন টার্মিনালটি একটি কল করে  ভার্চুয়াল এনভায়রনমেন্টটি এক্টিভেট করবে যার ফলে এটি টার্মিনালে লোড হয়ে আসবে। ভার্চুয়াল এনভায়রনমেন্টের নামটি (`.venv`)  প্রম্পটে দেখা যাবেঃ

    ```output
    ➜  nightlight source .venv/bin/activate
    (.venv) ➜  nightlight 
    ```

1. VS Code explorer থেকে `app.py` ফাইলটি ওপেন করি এবং নিম্নের কোডটি এড করিঃ

    ```python
    import json
    import time
    
    import paho.mqtt.client as mqtt
    
    id = '<ID>'
    
    client_telemetry_topic = id + '/telemetry'
    client_name = id + 'nightlight_server'
    
    mqtt_client = mqtt.Client(client_name)
    mqtt_client.connect('test.mosquitto.org')
    
    mqtt_client.loop_start()
    
    def handle_telemetry(client, userdata, message):
        payload = json.loads(message.payload.decode())
        print("Message received:", payload)
    
    mqtt_client.subscribe(client_telemetry_topic)
    mqtt_client.on_message = handle_telemetry
    
    while True:
        time.sleep(2)
    ```

    আমদের ডিভাইস বানানোর সময় যে ইউনিক আইডিটি ব্যবহার করেছিলাম তা ৬নং লাইনের `<ID>`-তে বসিয়ে দিই।

    ⚠️ এই আইডিটি **অব্যশই** আমাদের ডিভাইসে ব্যবহৃত একই আইডি হতে হবে নয়ত সার্ভার কোড সঠিক টপিক সাবস্ক্রাইব বা পাবলিশ কোনোটিই করবে না। 

    এই কোডটি একটি ইউনিক নামসহ একটি MQTT ক্লায়েন্ট তৈরি করে যা  * test.mosquitto.org * ব্রোকারের সাথে সংযুক্ত হয়। পরবর্তীতে এটি একটি প্রসেসিং লুপ চালু করে যেকোনো সাবস্ক্রাইব টপিকের মেসেজ লিসেনিং এর জন্য ব্যাকগ্রাউন্ড থ্রেডে রান করে  করে।

    ক্লায়েন্ট পরবর্তীতে টেলিমেট্রি টপিকের মেসেজগুলিতে সাবস্ক্রাইব করে এবং একটি ফাংশন সংজ্ঞায়িত করে আর যখন মেসেজ গৃহীত হয় তখনই ফাংশনটিকে কল করা হয়। যখন একটি টেলেমেট্রি মেসেজ গৃহীত হয় তখনই `handle_telemetry` ফাংশনটি কল করা হয় এবং কনসোলে গৃহীত বার্তাটি প্রিন্ট হয়।

    সবশেষে একটি ইনফিনিট লুপে অ্যাপ্লিকেশনটি রান হয়। ব্যাকগ্রাউন্ড থ্রেডে MQTT ক্লায়েন্ট বার্তাগুলোতে লিসেন করে এবং মেইন এপ্লিকেশনটি রান হওয়া অবস্থায় এটি  সবসময় রান হয়।

1. পাইথন এপটি রান করতে VS Code টার্মিনাল হতে নিম্নের কোডটি রান করিঃ

    ```sh
    python app.py
    ```

    এপটি আইওটি ডিভাইসের মেসেজসমূহ লিসেন করতে শুরু করবে।

1. আমাদের অব্যশই নিশ্চিত হতে হবে যে ডিভাইসটি রান করছে কিনা এবং টেলিমেট্রি মেসেজ সেন্ড করছে কিনা। বাহ্যিক বা ভার্চুয়াল ডিভাইস হতে শনাক্তকৃত লাইট লেভেলটিকে এডজাস্ট করি। গৃহীত বার্তাসমূহ নিচের মতো টার্মিনালে প্রিন্ট হবে।

    ```output
    (.venv) ➜  nightlight-server python app.py
    Message received: {'light': 0}
    Message received: {'light': 400}
    ```

    প্রেরিত মেসেজ গ্রহণের জন্য নাইটলাইট ভার্চুয়াল এনভায়রনমেন্টে app.py ফাইলটিকে অবশ্যই রান অবস্থায় থাকতে হবে।

> 💁 কোডটি [code-server/server](code-server/server) ফোল্ডারে পাওয়া যাবে।

### টেলিমেট্রি কতবার পাঠানো উচিত?

টেলিমেট্রিতে একটি গুরুত্বপূর্ণ বিষয় হল কতবার ডেটা পরিমাপ এবং প্রেরণ করা উচিত? উত্তরটি হল, এটি পরিস্থিতিভেদে নির্ভরশীল। যেমনঃ আমরা যদি প্রায়ই পরিমাপ করি তবে পরিমাপ  পরিবর্তনের প্রতি দ্রুত সারা দিতে পারবো কিন্তু আমরা যদি আরও পাওয়ার, ব্যান্ডউইথ, ডেটা ব্যাবহার করি তাহলে এইগুলো প্রসেস করার জন্য আরও ক্লাউড রিসোর্সের প্রয়োজন হবে যার ফলে আমাদের প্রায়ই যথেষ্ট পরিমাণে পরিমাপ করা প্রয়োজন তবে তা খুব বেশি নয়।

একটি থার্মোস্ট্যাট যদি কয়েক মিনিট পর পর তাপমাত্রা পরিমাপ করে তবে তা প্রয়োজনের অতিরিক্ত কারণ তাপমাত্রা প্রতিনিয়ত পরিবর্তিত হয় না। যদি আমরা দৈনিক একবার তাপমাত্রা পরিমাপ করি তবে ভরদুপুরে নাইটটাইমের তাপমাত্রায় আমাদের ঘরটিকে উত্তপ্ত করে বসবো। অপরপক্ষে আমরা যদি প্রতি সেকেন্ডে পরিমাপ করি তবে অপ্রয়োজনীয় সহস্রাধিক তাপমাত্রার পরিমাপের ডেটা তৈরি হবে যাতে ইউজারের ইন্টারনেট এবং ব্যান্ডউইথ বেশি ব্যবহৃত হবে  (যা লিমিটেড ব্যান্ডউইথ প্ল্যানে চলা ইউজারদের সমস্যা হতে পারে) এবং আরও পাওয়ার ব্যবহার করবে যা ব্যাটারি চালিত ডিভাইসের জন্য সমস্যা হতে পারে (যেমনঃ রিমোট সেন্সর) যার ফলে ক্লাউড প্রোভাইডারে কম্পিউটিং রিসোর্সের প্রসেসিং এবং স্টোরিং এর ব্যয় বেড়ে যাবে।

যদি কোন ফ্যাক্টরিতে কোন মেসিনারি ডেটা পর্যবেক্ষণ করা হয় এবং যদি এই মেসিনটি নষ্ট হয় তবে বিপজ্জনক ক্ষতি হতে পারে তার সাথে কয়েক মিলিয়ন ডলারের রাজস্বও নষ্ট হতে পারে। তাই এমন পরিস্থিতে প্রতি সেকেন্ডে একাধিকবার পরিমাপ করা অবশ্যই যুক্তিযুক্ত। টেলিমেট্রিযেকোন যন্ত্র নষ্ট হওয়ার আগে সেটিকে বন্ধ এবং ঠিক করা প্রয়োজন তা ইন্ডিকেট করে দেয়, তাই টেলিমেট্রি মিস হওয়ার চেয়ে ব্যান্ডউইথ নষ্ট হওয়া ভাল।

> 💁 এই পরিস্থিতিতে,ইন্টারনেটের উপর নির্ভরতা হ্রাস করতে প্রথম টেলিমেট্রি প্রসেস করার জন্য একটি এজ(edge) ডিভাইস রাখা যেতে পারে।

### লস অফ কানেক্টিভিটি

ইন্টারনেট সংযোগসমূহ হতে পারে অনির্ভরযোগ্য,বিভ্রাটপূর্ণ । এই অবস্থায় আইওটি ডিভাইসের কি করা উচিত – এটি কি ডেটাকে হারাতে দিবে নাকি কানেক্টিভিটি পুনরায় না আসা পর্যন্ত ডেটাকে স্টোর করে রাখবে? আবারো উত্তরটি হচ্ছে এটি পরিস্থিতিভেদে নির্ভরশীল।

একটি থার্মোস্ট্যাট এর নতুন তাপমাত্রা পরিমাপ করা মাত্রই আগের পরিমাপ করা ডেটাটি হারিয়ে যেতে পারে। ২০ মিনিট পূর্বে তাপমাত্রা ২০.৫°C ছিল বা এখন তাপমাত্রা ১৯°C তা নিয়ে হিটিং সিস্টেম পরোয়া করে না, বর্তমান মুহূর্তের তাপমাত্রাই নির্ধারণ করবে যে হিটিং সিস্টেমটি অন হবে নাকি অফ থাকবে।

মেশিনারির জন্য ডেটা রাখা যেতে পারে বিশেষত যদি এটি ট্রেন্ডস(trends) সন্ধানে ব্যবহৃত হয়। এমন কিছু মেশিন লার্নিং মডেল রয়েছে যা নির্ধারিত সময়ের মধ্যে(যেমন শেষ ঘন্টায়) ডেটা সন্ধান করে ডেটার স্ট্রিমসমূহে এনোম্যালি চিহ্নিত করে এনোম্যালাস ডেটা শনাক্ত করতে পারে। এটি প্রায়ই predictive maintenance এর জন্য ব্যবহৃত হয়, এমন কিছু লক্ষণের সন্ধান করে যাতে দ্রুত কোনও কিছু ভেঙে যাওয়ার আগেই এটি মেরামত বা রিপ্লেস করা যায় । এনোমলি ডিটেকশনের জন্য একটি মেশিনের প্রেরিত টেলিমেট্রিটির প্রতিটি বিটকে প্রসেস করা হয় তাই যখন আইওটি ডিভাইসটি পুনরায় ইন্টারনেটের সাথে সংযোগ স্থাপন করে তখন ইন্টারনেট আউটেজের সময় তৈরি হওয়া সমস্ত টেলিমেট্রি প্রেরণ করতে পারে।

আইওটি ডিভাইস ডিজাইনারদের বিবেচনা করা উচিত যাতে ইন্টারনেট আউটেজের সময় বা অবস্থানজনিত কারণে সিগন্যাল লসের সময় আইওটি ডিভাইস ব্যবহার করা যাবে কিনা। একটি স্মার্ট থার্মোস্ট্যাট যদি আউটেজের কারনে ক্লাউডে টেলিমেট্রি প্রেরণ করতে না পারে তবে হিটিং সিস্টেমকে কন্ট্রোল করতে তাতে অবশ্যই সীমিত সংখ্যক সিদ্ধান্ত নেওয়ার সক্ষমতা থাকতে হবে।

[![এই ফেরারীটি আচ্ছাদিত হয়ে আছে কারণ কেউ একজন আন্ডারগ্রাউন্ডে এটিকে আপগ্রেড করতে চেয়েছিল যেখানে কোনো সেল অপারেশন নেই ](../../../../images/bricked-car.png)](https://twitter.com/internetofshit/status/1315736960082808832)

লস অফ কানেক্টিভিটি MQTT-কতৃক সামালানোর জন্য প্রয়োজনে ডিভাইস এবং সার্ভার কোডকে মেসেজ ডেলিভারি নিশ্চিত করার দায়ভার গ্রহণ করতে হবে। উদারণস্বরূপ, একটি রিপ্লাই টপিকে অতিরিক্ত মেসেজসমূহের মাধ্যমে প্রেরিত সমস্ত মেসেজসমূহ প্রয়োজনে চাওয়া এবং তা যদি না হয় তবে সেগুলো ম্যানুয়ালি একটি সারিতে থাকবে যাতে পরবর্তী রিপ্লেতে দিবে।

## কমান্ডসমূহ

কমান্ডস হচ্ছে মেসেজ যা ক্লাউড দ্বারা কোন ডিভাইসে প্রেরণ করা হয় যাতে কিছু করার নির্দেশনা দেওয়া থাকে। বেশিরভাগক্ষেত্রে একচুয়েটরের আউটপুট সম্বলিত কিছু থাকে কিন্তু এতে ডিভাইসের নিজের জন্য কিছু নির্দেশনাও থাকতে পারে (যেমনঃ রিব্যুট করা) বা এক্সট্রা টেলিমেট্রি জড়ো করা এবং রেসপন্স হিসেবে কমান্ডকে রিটার্ন করা।

![ইন্টারনেটে সংযুক্ত একটি থার্মোস্ট্যাট কমান্ড রিসিভের মাধ্যমে হিটিং সিস্টেমকে চালু করছে](../../../../images/commands.png)

হিটিং সিস্টেম চালু করার জন্য একটি থার্মোস্ট্যাট ক্লাউ থেকে কমান্ড গ্রহণ করতে পারে। যদি ক্লাউড সার্ভিস সমস্ত সেন্সর হতে প্রাপ্ত টেলিমেট্রি ডাটার উপর ভিত্তি করে সিদ্ধান্ত নেয় যে হিটিং সিস্টেমটি চালু করা জরুরি তবে তা সে অনুযায়ী কমান্ড প্রেরণ করবে।

### কমান্ডসমূহ MQTT ব্রোকারে প্রেরণ

ইন্টারনেটের মাধ্যমে নাইটলাইটকে নিয়ন্ত্রনের পরবর্তী ধাপটি হলো সার্ভার কোড কতৃক আইওটি ডিভাইসে কমান্ড প্রেরণ করা যাতে লাইটের লেভেল উপলদ্ধি মাধ্যমে লাইটকে কন্ট্রোল করা যায়।

1. সার্ভার কোডটি VS Code এ ওপেন করি।

1. নিচের লাইনটি `client_telemetry_topic` ডিক্লেয়ারের পর এড করি যা নির্ধারণ করবে কোন টপিকে কমান্ড সেন্ড করবেঃ

    ```python
    server_command_topic = id + '/commands'
    ```

1. নিচের কোডটি `handle_telemetry` ফাংশেন শেষে এড করিঃ

    ```python
    command = { 'led_on' : payload['light'] < 300 }
    print("Sending message:", command)
    
    client.publish(server_command_topic, json.dumps(command))
    ```

    এই কোডটি একটি JSON মেসেজ  `led_on` এর ভ্যলুসহ কমান্ড টপিকে পাঠায় যা লাইটের ভ্যালু ৩০০ এর বেশি বা কমের উপর ভিত্তি করে তা ট্রু বা ফলস-এ সেট হয়। যদি লাইটের ভ্যালু (`led_on`<৩০০) ৩০০ এর কম হয় তবে ট্রু সেন্ড করা হয় যাতে এলইডি অন করার নির্দেশনা থাকে।

1. কোডটি পূর্বের মতো রান করি।

1. আমাদের বাহ্যিক বা ভার্চুয়াল ডিভাইসে কতৃক শনাক্তকৃত লাইটের লেভেল অনুসারে লেভেলটি এডজাস্ট করি। গ্রহীত মেসেজ এবং প্রেরিত কমান্ডগুলো টার্মিনালে আউটপুট হিসেবে বর্ণিত হবেঃ

    ```output
    (.venv) ➜  nightlight-server python app.py
    Message received: {'light': 0}
    Sending message: {'led_on': True}
    Message received: {'light': 400}
    Sending message: {'led_on': False}
    ```

> 💁 প্রতিটি সিংগেল টপিকে টেলিমেট্রি এবং কমান্ডসমূহ প্রেরণ করা হচ্ছে। যার অর্থ দাঁড়ায় একাধিক ডিভাইস থেকে টেলিমেট্রি একই টেলিমেট্রি টপিকের উপর প্রকাশিত হবে এবং একাধিক ডিভাইসের কমান্ডগুলিও একই কমান্ডের টপিকে প্রকাশিত হবে। যদি কোন নির্দিষ্ট ডিভাইসে কমান্ড প্রেরণ করতে চাই তবে একটি ইউনিক ডিভাইস আইডি নামকরণ (যেমনঃ `/commands/device1`, `/commands/device2`)  করে একাধিক টপিক ব্যবহার করে পারবো। এইভাবে কোন ডিভাইস কেবল সেই এক ডিভাইসের জন্য বরাদ্দকৃত বার্তাগুলি লিসেন করতে পারে।

> 💁 আমরা [code-commands/server](code-commands/server) এই ফোল্ডারে কোডটি পাবো।

### আইওটি ডিভাইসে কমান্ডসমূহের পরিচালনা করা

এখনে যেহেতু সার্ভার হতে কমান্ডসমূহ প্রেরিত হচ্ছে সেহেতু আমরা আইওটি ডিভাইসকে পরিচালনা করার জন্য এবং এলইডিকে নিয়ন্ত্রণের জন্য তাতে কোড এড করতে পারবো।

MQTT ব্রোকার হতে কমান্ডসমূহ গ্রহণের জন্য নিচের পদক্ষেপগুলো অনুসরণ করিঃ

* [আরডুইনো Wio টার্মিনাল](wio-terminal-commands.bn.md)
* [সিংগেল বোর্ড কম্পিউটার - রাস্পবেরি পাই/ভার্চুয়াল আইওটি ডিভাইস](single-board-computer-commands.bn.md)

এই কোডটি লেখা এবং রান করা হয়ে গেলে আমরা লাইটের লেভেল চেঞ্জ করে এক্সপেরিমেন্ট করবো। লাইটের লেভেল চেঞ্জের মাধ্যমে এলইডিটিতে এবং সার্ভার আর ডিভাইসের আউটপুটটিতে লক্ষ্য রাখি।

### লস অফ কানেক্টিভিটি

যদি আইওটি ডিভাইসে কমান্ড প্রেরণের প্রয়োজন হয় কিন্তু ডিভাইসটি অফলাইনে থাকে তবে এমতাবস্থায় ক্লাউড সার্ভিসের কি করা উচিত? আবারও, উত্তরটি হলো তা নির্ভরশীল।

যদি লেটেস্ট কমান্ডটি তার পূর্বের কমান্ডটিকে ওভাররাইট করে তবে পূর্বের কমান্ডটি উপেক্ষিত হবে। যদি কোন ক্লাউড সার্ভিস প্রথমে হিটিং সিস্টেমটি চালু করার জন্য একটি কমান্ড পাঠায় তারপর হিটিং সিস্টেমটি বন্ধ করার জন্য দ্বিতীয় আরেকটি কমান্ড পাঠায় তবে অন কমান্ডটি অর্থাৎ ১ম কমান্ডটি উপেক্ষা করা হবে এবং তা রিসেন্ট হবে না।

যদি কমান্ডসমূহের ক্রমানুসারে প্রসেসের প্র্য়োজন হয় যেমন হতে পারে প্রথমে একটি রোবটের হাত উপরে উঠানো দ্বিতীয়ত সেটির গ্র্যাবার বন্ধ করা, তাই কানেক্টিভিটি পুনরায় চালু হলে কমান্ডসমূহকে নিয়মানুযায়ী প্রেরণ করা প্রয়োজন।

✅ কীভাবে ডিভাইস বা সার্ভার কোডটি নিশ্চিত হবে যে কমান্ডসমূহ সর্বদা প্রেরিত হবে এবং প্রয়োজন পরলে তা MQTT-র মাধ্যমে নিয়মানুযায়ী প্রকাশিত হবে?

---

## 🚀 চ্যালেঞ্জ

শেষ তিনটি পাঠ্যের মধ্যে চ্যালেঞ্জটি ছিল আমাদের বাড়ি, স্কুল বা কর্মক্ষেত্রে যতগুলো আইওটি ডিভাইস রয়েছে তার একটি তালিকা তৈরি করা এবং তারা মাইক্রোকন্ট্রোলার বা একক-বোর্ড কম্পিউটার বা উভয়ের মিশ্রণে নির্মিত কিনা তার সিদ্ধান্তে উপনিত হওয়া এবং তারা কী ধরনের সেন্সর ও একচুয়েটর ব্যবহার করছে তা নিয়ে চিন্তা করা।

চিন্তা করে দেখি যে এই ডিভাইসগুলো কী ধরনের মেসেজ প্রেরণ বা গ্রহণ করছে। কি ধরনের টেলিমেট্রি প্রেরণ করছে? কি মেসেজ বা কমান্ড রিসিভ করতে পারে? চিন্তা করে দেখি এগুলো কি সত্যিই সুরক্ষিত?

## লেকচার পরবর্তী কুইজ

[লেকচার পরবর্তী কুইজ](https://brave-island-0b7c7f50f.azurestaticapps.net/quiz/8)

## রিভিউ এবং স্ব-অধ্যয়ন

[MQTT Wikipedia page](https://wikipedia.org/wiki/MQTT) টি পড়ে MQTT সম্পর্কে আরো জানতে পারবো।

[Mosquitto](https://www.mosquitto.org) ব্যবহার করে MQTT ব্রোকার রান করতে ট্রাই করি এবং এটিকে আইওটি ডিভাইস ও সার্ভার কোডের সাথে সংযুক্ত করি।

> 💁 টিপ – বাই ডিফল্ট Mosquitto কখনো anonymous কানেকশন অনুমোদন করে না (anonymous কানেকশনের অর্থ হচ্ছে ইউজারনেম এবং পাসওয়ার্ড ব্যাতীত কানেক্ট হওয়া) এবং যেই কম্পিউটারে এটি রান হচ্ছে সেই কম্পিউটার ব্যাতীত অন্য কানেকশন অনুমোদন করে না।
> এটিকে [`mosquitto.conf` config file](https://www.mosquitto.org/man/mosquitto-conf-5.html) এর মাধ্যমে ফিক্স করতে নিম্নের কমান্ডটি দিইঃ
>
> ```sh
> listener 1883 0.0.0.0
> allow_anonymous true
> ```

## এসাইনমেন্ট

[MQTT-এর সাথে অন্যান্য কমিউনিকেশন প্রটোকলের তুলনা করে পার্থক্য দাঁড় করানো](assignment.bn.md)