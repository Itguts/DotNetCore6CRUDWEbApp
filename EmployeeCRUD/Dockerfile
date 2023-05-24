#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["EmployeeCRUD/EmployeeCRUD.csproj", "EmployeeCRUD/"]
RUN dotnet restore "EmployeeCRUD/EmployeeCRUD.csproj"
COPY . .
WORKDIR "/src/EmployeeCRUD"
RUN dotnet build "EmployeeCRUD.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "EmployeeCRUD.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
# Set environment variables for Azure SQL Database
ENV ConnectionStrings__DefaultConnection="Server=tcp:yourserver.database.windows.net,1433;Initial Catalog=yourdatabase;Persist Security Info=False;User ID=yourusername;Password=yourpassword;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
ENTRYPOINT ["dotnet", "EmployeeCRUD.dll"]