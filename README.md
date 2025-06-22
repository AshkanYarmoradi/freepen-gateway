# FreePenGateway

FreePenGateway is a reverse proxy application built with ASP.NET Core and YARP (Yet Another Reverse Proxy) that provides a gateway to access [FreePen.net](https://www.freepen.net).

## Overview

This application acts as an intermediary between users and FreePen.net, forwarding requests to the target website while modifying request headers to ensure proper functionality. It can be used to:

- Provide alternative access to FreePen.net content
- Bypass certain network restrictions
- Create a customized access point for FreePen.net

## Features

- Full request forwarding to FreePen.net
- Automatic header modification to ensure proper routing
- HTTPS redirection for secure connections
- HTTP Strict Transport Security (HSTS) for enhanced security
- Configurable reverse proxy settings

## Prerequisites

- .NET 9.0 SDK or later
- A valid SSL certificate (for production deployments)

## Installation

1. Clone the repository:
   ```
   git clone https://github.com/yourusername/FreePenGateway.git
   cd FreePenGateway
   ```

2. Build the application:
   ```
   dotnet build
   ```

3. Run the application:
   ```
   dotnet run
   ```

The application will start and listen on the configured ports (by default, http://localhost:5000 and https://localhost:5001).

## Configuration

The application is configured through the `appsettings.json` file. The main configuration section is `ReverseProxy`, which defines the routes and clusters for the YARP reverse proxy.

### Routes Configuration

Routes define how incoming requests are matched and transformed before being forwarded to the destination.

```json
"Routes": {
  "all": {
    "ClusterId": "freepen_cluster",
    "Match": {
      "Path": "{**catch-all}"
    },
    "Transforms": [
      {
        "RequestHeader": "Host",
        "Set": "www.freepen.net"
      },
      {
        "RequestHeader": "Origin",
        "Set": "https://www.freepen.net"
      },
      {
        "RequestHeader": "Referer",
        "Set": "https://www.freepen.net/"
      }
    ]
  }
}
```

### Clusters Configuration

Clusters define the destination servers where requests will be forwarded.

```json
"Clusters": {
  "freepen_cluster": {
    "Destinations": {
      "destination1": {
        "Address": "https://www.freepen.net/"
      }
    }
  }
}
```

## Security Considerations

### Built-in Security Features

- **HTTPS Redirection**: All HTTP requests are automatically redirected to HTTPS.
- **HTTP Strict Transport Security (HSTS)**: Ensures that browsers always use HTTPS for future requests.
- **Exception Handling**: Production environments use a dedicated error handler to prevent leaking sensitive information.

### Additional Security Recommendations

1. **Rate Limiting**: Consider implementing rate limiting to prevent abuse.
2. **IP Filtering**: Restrict access to trusted IP addresses if the gateway is intended for specific users.
3. **Authentication**: Add authentication if the gateway should be accessible only to authorized users.
4. **Regular Updates**: Keep the application and its dependencies up to date to address security vulnerabilities.
5. **Logging and Monitoring**: Implement comprehensive logging and monitoring to detect and respond to suspicious activities.
6. **Content Security Policy**: Configure appropriate Content Security Policy headers to mitigate XSS attacks.

## Customization

### Adding Authentication

To add authentication to the gateway, modify the `Program.cs` file to include authentication services and middleware:

```csharp
// Add authentication services
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options => {
        // Configure JWT options
    });

// Add authorization services
builder.Services.AddAuthorization();

// In the request pipeline
app.UseAuthentication();
app.UseAuthorization();
```

### Implementing Rate Limiting

To add rate limiting, use the built-in rate limiting middleware in ASP.NET Core:

```csharp
// Add rate limiting services
builder.Services.AddRateLimiter(options =>
{
    options.GlobalLimiter = PartitionedRateLimiter.Create<HttpContext, string>(context =>
    {
        return RateLimitPartition.GetFixedWindowLimiter(
            partitionKey: context.Connection.RemoteIpAddress?.ToString() ?? "anonymous",
            factory: partition => new FixedWindowRateLimiterOptions
            {
                AutoReplenishment = true,
                PermitLimit = 100,
                QueueLimit = 0,
                Window = TimeSpan.FromMinutes(1)
            });
    });
});

// In the request pipeline
app.UseRateLimiter();
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Disclaimer

This application is provided for educational and legitimate use cases only. Users are responsible for ensuring their use of this tool complies with all applicable laws, regulations, and terms of service for the target website.