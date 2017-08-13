FROM microsoft/aspnetcore
WORKDIR /published
COPY ./published .
ENV ASPNETCORE_URLS http://+:5000
EXPOSE 5000
ENTRYPOINT ["dotnet", "DotNetCoreApplication.dll"]