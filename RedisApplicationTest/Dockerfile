#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-alpine AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-alpine AS build
WORKDIR /src
COPY ["RedisApplicationTest/RedisApplicationTest.csproj", "RedisApplicationTest/"]
RUN dotnet restore "RedisApplicationTest/RedisApplicationTest.csproj"
COPY . .
WORKDIR "/src/RedisApplicationTest"
RUN dotnet build "RedisApplicationTest.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "RedisApplicationTest.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RedisApplicationTest.dll"]