# jwt

## يجب اجراء التعديلات التالية لانشاء مشروع يتوافق مع تقنية JWT
### اضافة المكتبات التالية
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
### يجب اضافة المعلومات الايميل و التوغين في ملف  appsettings.cs
#####  //JWT code
#####  "ConnectionStrings": {
#####    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=JWTDataBase;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=False",
#####  },
#####    "JWT": {
#####      "Key": "gtJmzN5tWiXZX0f1pwHur9tn6p3PtPMzXQ6pNWnmV+o=",
#####      "Issuer": "SecureApi",
#####      "Audience": "SecureApiUser",
#####      "DurationInDays": 3
#####    },
#####    "EmailSender": {
#####      "Host": "smtp.gmail.com",
#####      "Port": 587,
#####      "EnableSSL": true,
#####      "UserName": "admin@test.com",
#####      "Password": "Qweasd12#"
#####    },
#####    //JWT code
### يجب نسخ الكود المحدد بتاغ ج و ت الموجود في ملف Program.cs 
##### //jwt code
##### builder.Services.Configure<JWTValues>(builder.Configuration.GetSection("JWT"));
##### builder.Services.AddDbContext<ApplicationDbContext>(
#####     opt => opt.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
##### builder.Services.AddAuthentication(options =>
##### {
#####     options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
#####     options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
##### }).AddJwtBearer(opt => {
#####     opt.RequireHttpsMetadata = false;
#####     opt.SaveToken = false;
#####     opt.TokenValidationParameters = new TokenValidationParameters
#####     {
#####        ValidateIssuerSigningKey = true,
#####        ValidateIssuer = true,
#####        ValidateAudience = true,
#####        ValidateLifetime = true,
#####        ValidIssuer = builder.Configuration["JWT:Issuer"],
#####        ValidAudience = builder.Configuration["JWT:Audience"],
#####        IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(builder.Configuration["JWT:Key"]!))
#####    };
##### });
##### builder.Services.AddIdentity<IdentityUser, IdentityRole>(opt =>
##### {
#####     //SingIn
#####    opt.SignIn.RequireConfirmedEmail = false;
#####    opt.SignIn.RequireConfirmedPhoneNumber = false;
#####    opt.SignIn.RequireConfirmedAccount = false;
#####    //Password
#####    opt.Password.RequireDigit = false;
#####   opt.Password.RequiredLength = 6;
#####    opt.Password.RequiredUniqueChars = 0;
#####    opt.Password.RequireNonAlphanumeric = false;
#####    opt.Password.RequireUppercase = false;
#####    opt.Password.RequireLowercase = false;
##### }).AddEntityFrameworkStores<ApplicationDbContext>().AddDefaultTokenProviders();
##### builder.Services.AddTransient<IAuthServies, AuthServies>();
##### builder.Services.AddTransient<IEmailSender, EmailSender>(a =>
#####              new EmailSender(
#####                  builder.Configuration["EmailSender:Host"]!,
#####                  builder.Configuration.GetValue<int>("EmailSender:Port"),
#####                  builder.Configuration.GetValue<bool>("EmailSender:EnableSSL"),
#####                  builder.Configuration["EmailSender:UserName"]!,
#####                  builder.Configuration["EmailSender:Password"]!
#####              )
#####          );
##### //jwt code
  
##### //JWT Add defulte role and user admin
##### await AddDatabaseItem.AddRoll(app.Services, new List<string> { "User", "Admin", "Employee" });
##### await AddDatabaseItem.AddAdmin(app.Services, builder.Configuration["EmailSender:UserName"]!);
##### //JWT
##### app.Run();
  
### لحماية الكنترول من الدخول غير المصرح نضع  [Authorize(AuthenticationSchemes = "Bearer")]
### كما يمكن اضافة الصلاحيات مثال [Authorize(AuthenticationSchemes = "Bearer",Roles = "User")]
### يجب اضافة جدول refreshToken to applicationDbContex
