

### 基础配置

```csharp

using Serilog.Events;
using Serilog;

// Serilog Config
var loggerConfiguration = new LoggerConfiguration()
   .MinimumLevel.Override("Microsoft", LogEventLevel.Information)
   .MinimumLevel.Override("Microsoft.EntityFrameworkCore", LogEventLevel.Warning)
   .Enrich.FromLogContext()
   .WriteTo.Console(outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss} [{Level:u3}] {Message:lj}{NewLine}{Exception}")
   .WriteTo.File("Logs/.log", rollingInterval: RollingInterval.Day, outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss.fff} [{Level:u3}] {SourceContext} {ThreadId}  {Message:lj}{NewLine}{Exception}", retainedFileTimeLimit: TimeSpan.FromDays(7));

Log.Logger = loggerConfiguration.CreateLogger();

var builder = WebApplication.CreateBuilder(args);

 // Add Serilog
 builder.Host.UseSerilog();

  var app = builder.Build();

 // Configure the HTTP request pipeline.
 if (!app.Environment.IsDevelopment())
 {
     app.UseExceptionHandler("/Error");
 }
 app.UseStaticFiles();

 app.UseRouting();

 app.UseAuthorization();

 app.UseSession();
 app.MapRazorPages();
 app.MapControllers();

 app.Run();

```

