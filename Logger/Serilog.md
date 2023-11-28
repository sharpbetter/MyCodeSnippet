
- 程序配置
    ``` csharp

            var loggerConfiguration = new LoggerConfiguration()
    #if DEBUG
                .MinimumLevel.Debug()
                .MinimumLevel.Override("Volo.Abp", LogEventLevel.Information)
    #else
                .MinimumLevel.Information()
    #endif
                .MinimumLevel.Override("OpenIddict", LogEventLevel.Warning)
                .MinimumLevel.Override("Microsoft", LogEventLevel.Information)
                .MinimumLevel.Override("Microsoft.EntityFrameworkCore", LogEventLevel.Warning)
                .Enrich.FromLogContext()
                .WriteTo.Async(c => c.File("Logs/.log", rollingInterval: RollingInterval.Day, outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss.fff} [{Level:u3}] {SourceContext} {ThreadId}  {Message:lj}{NewLine}{Exception}", retainedFileTimeLimit: TimeSpan.FromDays(7)))
                .WriteTo.Async(c => c.Console(outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss} [{Level:u3}] {Message:lj}{NewLine}{Exception}"));

            if (IsMigrateDatabase(args))
            {
                loggerConfiguration.MinimumLevel.Override("Volo.Abp", LogEventLevel.Warning);
                loggerConfiguration.MinimumLevel.Override("Microsoft", LogEventLevel.Warning);
            }

            Log.Logger = loggerConfiguration.CreateLogger();

    ```

