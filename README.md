# jwt

## يجب اجراء التعديلات التالية لانشاء مشروع يتوافق مع تقنية JWT  
###  أولاً:اضف المكتبات التالية من Nuget
##### Microsoft.AspNetCore.Authentication.JwtBearer
##### Microsoft.AspNetCore.Authentication.OpenIdConnect
##### Microsoft.AspNetCore.Identity.EntityFrameworkCore
##### Microsoft.AspNetCore.Identity.UI
##### Microsoft.AspNetCore.OpenApi
##### Microsoft.EntityFrameworkCore
##### Microsoft.EntityFrameworkCore.Design
##### Microsoft.EntityFrameworkCore.SqlServer
##### Microsoft.EntityFrameworkCore.Tools
##### Microsoft.Identity.Web
##### Microsoft.VisualStudio.Web.CodeGeneration.Design
##### Swashbuckle.AspNetCore
##### System.IdentityModel.Tokens.Jwt

### ثانياً: انسخ الملف التالي AuthenticationController الى مجلد Controllers

### ثالثاً: اضف معلومات الايميل و التوغين على الشكل التالي في ملف  appsettings.cs

##### "JWT": {
#####       "Key": "gtJmzN5tWiXZX0f1pwHur9tn6p3PtPMzXQ6pNWnmV+o=",
#####       "Issuer": "SecureApi",
#####       "Audience": "SecureApiUser",
#####       "DurationInDays": 3
#####     },
#####     "EmailSender": {
#####       "Host": "smtp.gmail.com",
#####       "Port": 587,
#####       "EnableSSL": true,
#####       "UserName": "admin@test.com",
#####       "Password": "Qweasd12#"
#####     },

### رابعاً: انسخ الكود التالي إلى ملف Program.cs 

##### Seed.Setting(builder);    //Add this line

##### var app = builder.Build();

##### await Seed.AddRoll(app.Services, new List<string> { "User", "Admin", "Employee" });   //Add this line to add rolles 
##### await Seed.AddAdmin(app.Services, builder.Configuration["EmailSender:UserName"]!);    //Add this line to add admin user

### خامساً: اضف جدول refreshToken إلى applicationDbContex

## ملاحظات
#### لحماية الكنترول من الدخول غير المصرح نضع  [Authorize(AuthenticationSchemes = "Bearer")]
#### كما يمكن اضافة الصلاحيات مثال [Authorize(AuthenticationSchemes = "Bearer",Roles = "User")]

