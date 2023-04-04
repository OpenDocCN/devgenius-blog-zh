# 应用程序最简单的后端。

> 原文：<https://blog.devgenius.io/the-simplest-backend-for-your-application-6bee19074818?source=collection_archive---------9----------------------->

连接到数据库并管理它对初学者来说并不容易！

*   编写您的后端
*   测试它
*   部署它

在另一端部署需要进一步了解云。

作为一名后端开发人员，我通常用 python 和一个名为 [flask](https://flask.palletsprojects.com/en/2.0.x/) 的微框架编写后端，部署我在脚本中所做的一个字母的更改是非常困难和耗时的。

但是现在不一样了，集成后端简化为集成 API！

CSV2API 是一个微小的项目，帮助 noobs 将数据库与初学者知识相结合。

**我们是这样做的**

*   数据库顾名思义就是我们需要的数据，它通常存储在云存储中。在这里，它是一个手动存储在我们的存储库或任何其他在线存储提供商的 CSV 文件。
*   数据库中最基本的先决条件是**查询**，而 CSV2API 支持这一点。
*   在本教程中，我们将使用这个[样本](https://github.com/dev-Roshan-lab/healthcare/blob/main/data.csv)数据集，它讨论了一系列医生及其在各自领域的工作。

因此，对于已经为我们创建的最简单的后端，用 python 编程语言编写的一个简单的 API 请求应该是:

输出:

```
{"results":[{" meet_id":" null"," tags":"Throat problem*Ear problem*Ear Nose Throat Diseases*Microscopic Ear Surgery*Endoscopic Nose & Paranasal Sinus Surgery ( Fess)*Microscopic Laryngoscopic Surgery*Sleep Disorders*Hearing Aids*Sinus Surgery*Ear Infection","description":"Dr. Sanjay Sachdeva is one of the best ENT Surgeons in Delhi with specialization in Otorhinolaryngology. He has a rich experience of 32 years in the field of ENT and Head and Neck Surgery. He is recognized as an expertise in the area of Endoscopic Minimally Invasive Approach for Skull Base Surgery.","goodReviews":79.2,"image":"assets/doctor.png","isfavourite":true,"name":"Dr. Sanjay Sachdeva","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"Otolaryngologist"},{" meet_id":" null"," tags":"Cancer treatment*Urological Cancer Surgery","description":"Dr. Gagan Gautam is prominent name in the field of Uro Oncology and Robotic Surgery in India. He is a qualified surgeon who has obtained his specialization degree (MCh) from the prestigious All India Institute of Medical Sciences (AIIMS) and US fellowship in Robotic Uro oncology from University of Chicago a leading cancer center in the world. He specializes in the surgical treatment of prostate kidney bladder and other urinary tract cancers.","goodReviews":93.2,"image":"assets/doctor1.png","isfavourite":false,"name":"Dr. Gagan Gautam","rating":3.5,"satisfaction":89.2,"totalScore":72.2,"type":"Uro - Oncologist"},{" meet_id":" null"," tags":"Spinal Surgeries*Endoscopic Disc surgery*Disc replacement*Endoscopic Cranial Surgery*Brain Tumour Surgery*Minimally Invasive Surgery for Spinal Tumours*Motion preserving surgery","description":"Dr. Bipin Walia is one of the Best Neurosurgeons in New Delhi with more than two decades of vast clinical experience in the field of neuro and spine surgery. Dr. Walia has performed more than 7000 spinal surgeries and has a keen interest in advanced technique - Minimally Invasive Surgery for spinal tumors. He is recognized as a pioneer in endoscopic disc surgery and is an expert in the area of motion preserving surgery disc replacement surgery Image guided surgery endoscopic cranial surgery and brain tumor surgery.","goodReviews":88.2,"image":"assets/doctor3.png","isfavourite":false,"name":"Dr. Bipin S Walia","rating":2.5,"satisfaction":78.2,"totalScore":93.94,"type":"Neurosurgeon"},{" meet_id":" skr-lks-tmo"," tags":"Cavity filling*Root canal*Cosmetic Dentistry","description":"Dr. Surbhi Anand is a practicing Endodontist and Conservative Dentist with over 7 years of experience after procuring the bachelors degree she pursued a masters course in Endodontology specializing in Single sitting Root Canal Treatment cosmetic dentistry and smile makeover. She has been associated as a senior consultant with healthcare setups including Fortis hospital and Columbia Asia Hospital and has a vast consultation base to various reputable dental practices in Delhi/NCR.","goodReviews":12.2,"image":"assets/doctor2.png","isfavourite":true,"name":"Dr. Surbhi Anand","rating":1.5,"satisfaction":84.2,"totalScore":75.2,"type":"Dentist"},{" meet_id":" null"," tags":"Gastrointestinal*Gynae Cancers","description":"Dr. Sandeep Batra is Senior Consultant of Medical Oncology at the Max Super Speciality Hospital New Delhi. His educational qualifications include an MBBS from PGI Rohtak; an MD in Medicine from the same institute; a DNB in Medicine from the National Board of Examinations Delhi; and a PGDMLS from the Symbiosis University Pune. He was also previously a Senior Consultant at the Medical Oncology at Medanta The Medicity Gurgaon. He was also involved in academic activities including training of oncology students and oncology nurses. Dr. Batra has many national and international publications to his name and has also edited various oncology-related chapters in numerous books and online resources.","goodReviews":79.2,"image":"assets/doctor4.png","isfavourite":true,"name":"Dr. Sandeep Batra","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"Oncologist"},{" meet_id":" null"," tags":"Movement Disorders with Deep*Brain Stimulation (DBS)*Surgery for Epilepsy*Nerve and Brachial Plexus Surgery*Radiosurgery as well as Cerebrovascular Surgery","description":"Dr. Aditya Gupta is highly knowledgeable and skillful neurosurgeon presently practicing in the World's renowned Artemis Hospital Gurgaon. Cofounded the Institute of Neurosciences at Medanta Hospital. A graduate and topper from All India Institute of Medical Sciences (AIIMS) Dr. Aditya Gupta is an accomplished and experienced neurosurgeon. As a student resident and later Faculty at AIIMS he has won several awards such as the prestigious Sir Dorabji Tata Award. He was Associate Professor of Neurosurgery at AIIMS till late 2009.","goodReviews":79.2,"image":"assets/doctor5.png","isfavourite":true,"name":"Dr. Aditya Gupta","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"Neurosurgeon"},{" meet_id":" null"," tags":"Joint Replacement Surgery*Hip Arthroplasty*Knee Revisison surgery*Partial Knee Replaceement*Total Knee Replacement surgery*Trauma Management based on AO Principles","description":"Dr. Sanjeev Kumar Singh Marya is one of the Top Orthopedic Surgeons in India and carries over 35 years of extensive and rich experience in his field of specialization.He is a prominent orthopedic surgeon and is highly acknowledged for performing more than 15000 joint replacements surgeries with over 3500 simultaneous knee replacement and 3000 hip replacement surgeries in his illustrious career.","goodReviews":79.2,"image":"assets/doctor6.png","isfavourite":true,"name":"Dr. SKS Marya","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"Orthopaedic surgeon"},{" meet_id":" null"," tags":"Implant Dentistry*Immediate Implants*Management Of Implant Related Complications*Flap Surgeries*Root Coverage Procedures*Alveolar Ridge Augmentation","description":"An ambitious qualified Dentist with excellent organizational and interpersonal skills keen to render quality dental care in progressive dental practice. Dr. Amandeep Dhillon is a practicing Periodontist & Oral Implantologist with over 8 years of experience he graduated with bachelors degree in dental sciences with distinction/honor in the subject of Periodontology and was awarded certificate of merit in oral surgery conservative dentistry oral medicine and also holds a masters degree in Periodontology & Oral Implantology and holds a masters degree in Periodontology & Oral Implantology.","goodReviews":79.2,"image":"assets/doctor7.png","isfavourite":true,"name":"Dr. Amandeep Singh Dhillon","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"Dentist"},{" meet_id":" null"," tags":"Lymphoma & Leukemia*Head & Neck and Brain Tumors*Multiple Myeloma*Breast cancer treatment*Lung cancer treatment*Gastrointestinal and Gynecological tumors","description":"Dr. Nidhi Rawal has extensive experience in Pediatric and neonatal echo Transesophageal echocardiography and had been actively involved in the Cath lab interventions at PGI Chandigarh.Dr. Rawal has keen interest in research and has attended various national conferences and CMEs regularly. She has many publications (both national and international) to her credit.","goodReviews":79.2,"image":"assets/doctor10.png","isfavourite":true,"name":"Dr. Nidhi Rawal","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"Cardiologist"},{" meet_id":" null"," tags":"Voice disorders*Phonosurgery*Head and Neck Surgery*Lasers*Endoscopic sinus surgery*Sleep medicine sialology*Otology*Skull base surgery*Cochlear implants","description":"Dr. K. K. Handa is a renowned ENT specialist andOtorhinolaryngologist in India with over 27 years of rich clinical experience in this field. He is considered one of the best laryngologist voice surgeon and cochlear implant surgeon along with expertise in laser surgery endoscopic sinus surgery sleep medicine robotic surgery and head and neck surgery.","goodReviews":79.2,"image":"assets/doctor9.png","isfavourite":true,"name":"Dr. K K Handa","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"ENT Specialist"}]}
```

这样一个简单的 API 请求可以帮助我们获得一个 JSON 字符串，这个字符串可以根据需要进一步使用。

**让我们理解代码:**

*   首先，我们导入用于发送 API 请求的[请求](https://pypi.org/project/requests/)。
*   我们定义了发送一个 **GET** 请求所需的变量。将 **YOUR-RAPIDAPI-KEY** 替换为注册免费 [API](https://rapidapi.com/dev.vividgoat/api/csv2api1/) 时获得的密钥。
*   然后，我们向 API 发送一个 **GET** 请求。
*   我们需要记住的一点是，请求 url 中的 **url** 参数需要一个原始的 CSV 文件 URL，这与 Github 中任何 CSV 文件的普通链接不同。
*   我们是这样得到它的:

![](img/f440068fe47738d65509d140d37d3ccc.png)

*   最后，我们打印所请求的内容！

**查询**

查询在访问和接收数据时非常重要，如果没有它，当应用程序扩展时可能会导致许多麻烦！

*   接收大量数据会使应用程序变慢。
*   可能会给云提供商带来不必要的费用。
*   解析 JSON 会变得更加困难。
*   将更改引入数据将消耗更多 CPU。
*   使得开发人员更难处理数据。

幸运的是，我们在 CSV2API 中支持查询

我们是这样做的

我们传递一个名为`query`的 URL 参数，例如在我们的样本数据集中，我们有名为**的列，名称、类型、评级、好评、总分、满意度、isfavourite、图片、描述**

我们可以通过这样的方式进行查询，即我们只获得具有特定**等级**或**类型**等的医生的数据。

在上面的代码片段中，我们传递给数据库的查询是
`&query= name == 'Dr. Sanjay Sachdeva' and rating == 4.5`，在这里我们请求 API 返回数据，其中医生的名字是 Dr. Sanjay Sachdeva，他的评分是 4.5。

输出:

```
{"results":[{" meet_id":" null"," tags":"Throat problem*Ear problem*Ear Nose Throat Diseases*Microscopic Ear Surgery*Endoscopic Nose & Paranasal Sinus Surgery ( Fess)*Microscopic Laryngoscopic Surgery*Sleep Disorders*Hearing Aids*Sinus Surgery*Ear Infection","description":"Dr. Sanjay Sachdeva is one of the best ENT Surgeons in Delhi with specialization in Otorhinolaryngology. He has a rich experience of 32 years in the field of ENT and Head and Neck Surgery. He is recognized as an expertise in the area of Endoscopic Minimally Invasive Approach for Skull Base Surgery.","goodReviews":79.2,"image":"assets/doctor.png","isfavourite":true,"name":"Dr. Sanjay Sachdeva","rating":4.5,"satisfaction":85.2,"totalScore":93.2,"type":"Otolaryngologist"}]}
```

正如我们所看到的，数量急剧减少，理解也比以前容易多了。

**注意事项**

*   该字符串必须用单引号括起来
*   布尔值必须大写。`False`是正确的`false`是错误的。
*   整数或浮点数应该用单引号或双引号括起来。

在 [RapidAPI](https://rapidapi.com/dev.vividgoat/api/csv2api1/) 测试 API 的更多列和不同数据。

有疑问或面临错误？让我看看你在讨论中得到了什么