FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 5004

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["src/Services/Weixin/MsSystem.Weixin.API/MsSystem.Weixin.API.csproj", "src/Services/Weixin/MsSystem.Weixin.API/"]
COPY ["src/Services/Weixin/MsSystem.Weixin.IRepository/MsSystem.Weixin.IRepository.csproj", "src/Services/Weixin/MsSystem.Weixin.IRepository/"]
COPY ["src/Services/Weixin/MsSystem.Weixin.Model/MsSystem.Weixin.Model.csproj", "src/Services/Weixin/MsSystem.Weixin.Model/"]
COPY ["src/Services/Weixin/MsSystem.Weixin.ViewModel/MsSystem.Weixin.ViewModel.csproj", "src/Services/Weixin/MsSystem.Weixin.ViewModel/"]
COPY ["src/BuildingBlocks/Zipkin/JadeFramework.Zipkin/JadeFramework.Zipkin.csproj", "src/BuildingBlocks/Zipkin/JadeFramework.Zipkin/"]
COPY ["src/BuildingBlocks/Resilience/Resilience.Http.DependencyInjection/Resilience.Http.DependencyInjection.csproj", "src/BuildingBlocks/Resilience/Resilience.Http.DependencyInjection/"]
COPY ["src/BuildingBlocks/Resilience/Resilience.Http/Resilience.Http.csproj", "src/BuildingBlocks/Resilience/Resilience.Http/"]
COPY ["src/Services/Weixin/MsSystem.Weixin.IService/MsSystem.Weixin.IService.csproj", "src/Services/Weixin/MsSystem.Weixin.IService/"]
COPY ["src/Services/Weixin/MsSystem.Weixin.Repository/MsSystem.Weixin.Repository.csproj", "src/Services/Weixin/MsSystem.Weixin.Repository/"]
COPY ["src/Services/Weixin/MsSystem.Weixin.Service/MsSystem.Weixin.Service.csproj", "src/Services/Weixin/MsSystem.Weixin.Service/"]
RUN dotnet restore "src/Services/Weixin/MsSystem.Weixin.API/MsSystem.Weixin.API.csproj"
COPY . .
WORKDIR "/src/src/Services/Weixin/MsSystem.Weixin.API"
RUN dotnet build "MsSystem.Weixin.API.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "MsSystem.Weixin.API.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "MsSystem.Weixin.API.dll"]