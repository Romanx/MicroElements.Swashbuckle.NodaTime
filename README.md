# MicroElements.Swashbuckle.NodaTime
Allows configure Asp.Net Core and swagger to use NodaTime types.

## Latest Builds, Packages
[![License](http://img.shields.io/:license-mit-blue.svg)](https://raw.githubusercontent.com/micro-elements/MicroElements.Swashbuckle.NodaTime/master/LICENSE)
[![NuGet](https://img.shields.io/nuget/v/MicroElements.Swashbuckle.NodaTime.svg)](https://www.nuget.org/packages/MicroElements.Swashbuckle.NodaTime)
![NuGet](https://img.shields.io/nuget/dt/MicroElements.Swashbuckle.NodaTime.svg)
[![MyGet](https://img.shields.io/myget/micro-elements/v/MicroElements.Swashbuckle.NodaTime.svg)](https://www.myget.org/feed/micro-elements/package/nuget/MicroElements.Swashbuckle.NodaTime)

[![Travis](https://img.shields.io/travis/micro-elements/MicroElements.Swashbuckle.NodaTime/master.svg?logo=travis)](https://travis-ci.org/micro-elements/MicroElements.Swashbuckle.NodaTime)
[![AppVeyor](https://img.shields.io/appveyor/ci/micro-elements/microelements-swashbuckle-nodatime.svg?logo=appveyor)](https://ci.appveyor.com/project/micro-elements/microelements-swashbuckle-nodatime)
[![Coverage Status](https://img.shields.io/coveralls/micro-elements/MicroElements.Swashbuckle.NodaTime.svg)](https://coveralls.io/r/micro-elements/MicroElements.Swashbuckle.NodaTime)

[![Gitter](https://img.shields.io/gitter/room/micro-elements/MicroElements.Swashbuckle.NodaTime.svg)](https://gitter.im/micro-elements/MicroElements.Swashbuckle.NodaTime)

## Installation

### Package Reference:

```
dotnet add package microelements.swashbuckle.nodatime
```

## Getting started
- Add package reference to MicroElements.Swashbuckle.NodaTime
- Configure asp net core to use swagger
- Configure JsonSerializer to properly serialize NodaTime types. see `AddJsonFormatters` or `AddJsonOptions`
- Configure `AddSwaggerGen` with `ConfigureForNodaTime`

## Benefits of MicroElements.Swashbuckle.NodaTime
- Implemented in c#, no FSharp.Core lib in dependencies
- JsonSerializerSettings ContractResolver uses for NamingStrategy, so you can use DefaultNamingStrategy, CamelCaseNamingStrategy or SnakeCaseNamingStrategy
- Added new DateInterval (use NodaTime.Serialization.JsonNet >= 2.1.0)

## Sample
```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        void InitJsonSettings(JsonSerializerSettings serializerSettings)
        {
            // Use DefaultContractResolver or CamelCasePropertyNamesContractResolver;
            serializerSettings.ContractResolver = new DefaultContractResolver();

            // Configure JsonSerializer to properly serialize NodaTime types.
            serializerSettings.ConfigureForNodaTime(DateTimeZoneProviders.Tzdb);
        }

        // CASE1: AddMvcCore with AddJsonFormatters
        services
            .AddMvcCore()
            .AddApiExplorer()
            .AddJsonFormatters(InitJsonSettings)
            ;

        // CASE2: AddMvc with AddJsonOptions
        //services
        //    .AddMvc()
        //    .AddJsonOptions(options => InitJsonSettings(options.SerializerSettings))
        //    ;

        // Adds swagger
        services.AddSwaggerGen(c =>
        {
            c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });

            // Configures swagger to use NodaTime.
            // c.ConfigureForNodaTime();

            // Configures swagger to use NodaTime. Use the same InitJsonSettings action that in AddJsonFormatters
            c.ConfigureForNodaTime(InitJsonSettings);

            // Configures swagger to use NodaTime with serializerSettings.
            // c.ConfigureForNodaTime(serializerSettings);
        });
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseMvc().UseSwagger();

        // Adds swagger UI
        app.UseSwaggerUI(c =>
        {
            c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
        });
    }
}
```

## How it works
1. MicroElements.Swashbuckle.NodaTime creates Schemas for all NodaTime types
2. MicroElements.Swashbuckle.NodaTime configures JsonSerializer for examples
3. Maps types to [ISO 8601]

## Screenshots

## Without MicroElements.Swashbuckle.NodaTime
![](https://raw.githubusercontent.com/micro-elements/MicroElements.Swashbuckle.NodaTime/master/images/NodaTime0.png)

## With MicroElements.Swashbuckle.NodaTime
![](https://raw.githubusercontent.com/micro-elements/MicroElements.Swashbuckle.NodaTime/master/images/NodaTime1.png)

## With MicroElements.Swashbuckle.NodaTime (camelCase)
![](https://raw.githubusercontent.com/micro-elements/MicroElements.Swashbuckle.NodaTime/master/images/NodaTime2.png)

## Build
Windows: Run `build.ps1`

Linux: Run `build.sh`

## License
This project is licensed under the MIT license. See the [LICENSE] file for more info.

[LICENSE]: https://raw.githubusercontent.com/micro-elements/MicroElements.Swashbuckle.NodaTime/master/LICENSE
[ISO 8601]: https://xml2rfc.tools.ietf.org/public/rfc/html/rfc3339.html#anchor14
