FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["Directory.Build.props","."]
COPY ["Directory.Packages.props","."]
COPY ["src/DevHabit.Api/DevHabit.Api.csproj","DevHabit.Api/"]

RUN dotnet restore "DevHabit.Api/DevHabit.Api.csproj"

COPY src/ .


RUN dotnet build "DevHabit.Api/DevHabit.Api.csproj" -c ${BUILD_CONFIGURATION} -o /app/build


FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "DevHabit.Api/DevHabit.Api.csproj" -c ${BUILD_CONFIGURATION} -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
#ENTRYPOINT [ "dotnet","DevHabit.Api.dll" ]
CMD ["dotnet", "DevHabit.Api.dll"]
