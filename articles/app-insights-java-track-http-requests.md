<properties 
	pageTitle="Track HTTP requests in a Java web application" 
	description="Application Insights lets you measure performance of your web Java web application" 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="keboyd"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/02/2015" 
	ms.author="awills"/>
 
# Track HTTP requests in a Java web application

If you’re running a Java web application, you can view information about the HTTP requests sent to your application, such as the requested resources, failed requests and response times, all in the Application Insights portal.

Install [Application Insights SDK for Java][java], if you haven't already done that.


## Add the binaries to your project

*Choose the appropriate way for your project.*

### If you're using Maven...

If your project is already set up to use Maven for build, merge the following snippet of code to your pom.xml file.

Then refresh the project dependencies, to get the binaries downloaded.

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <version>[0.9,)</version>
      </dependency>
    </dependencies>

### If you're using Gradle...

If your project is already set up to use Gradle for build, merge the following snippet of code to your build.gradle file.

Then refresh the project dependencies, to get the binaries downloaded.

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '0.9.+'
    }

## Add the Application Insights HTTP filter to your project

Locate and open the web.xml file in your project, and merge the following snippet of code under the web-app node, where your application filters are configured.

To get the most accurate results, the filter should be mapped before all other filters.

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

## Add the HTTP modules to your project

Locate and open the ApplicationInsights.xml file in your project, and merge the following snippet of code under the <TelemetryModules> element.

If there is no <TelemetryModules> element in this file, add one under the <ApplicationInsights> element.

    <TelemetryModules>
      <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
      <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
      <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
    </TelemetryModules>

## Add telemetry initializers for events correlation

With events correlation, you can associate between an HTTP request and all the telemetry events that were sent during the request processing, using an Operation ID property that is attached to each of these telemetry events. This enables to explore an HTTP request together with all of the events that were called from it, and facilitates diagnosing and troubleshooting issues.

Locate and open the ApplicationInsights.xml file in your project, and merge the following snippet of code under the <TelemetryInitializers> element.

If there is no < TelemetryInitializers> element in this file, add one under the <ApplicationInsights> element.

    <TelemetryInitializers>
      <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
      <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    </TelemetryInitializers>


## View the requests information in Application Insights

Run your application.

Return to your Application Insights resource in Microsoft Azure.

HTTP requests data will appear on the overview blade. (If it isn't there, wait a few seconds and then click Refresh.)

![](./media/app-insights-java-track-http-requests/5-results.png)
 

Click through any chart to see more detailed metrics. 

![](./media/app-insights-java-track-http-requests/6-barchart.png)


[Learn more about metrics.][metrics]

 

And when viewing the properties of a request, you can see the telemetry events associated with it such as requests and exceptions.
 
![](./media/app-insights-java-track-http-requests/7-instance.png)




## Next steps

* [Search events and logs][diagnostic] to help diagnose problems.
* [Capture Log4J or Logback traces][javalogs]



<!--Link references-->

[alerts]: app-insightss-alerts.md
[android]: https://github.com/Microsoft/AppInsights-Android
[api]: app-insights-custom-events-metrics-api.md
[apiproperties]: app-insights-custom-events-metrics-api.md#properties
[apiref]: http://msdn.microsoft.com/library/azure/dn887942.aspx
[availability]: app-insights-monitor-web-app-availability.md
[azure]: insights-perf-analytics.md
[azure-availability]: insights-create-web-tests.md
[azure-usage]: insights-usage-analytics.md
[azurediagnostic]: insights-how-to-use-diagnostics.md
[client]: app-insights-web-track-usage.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[data]: app-insights-data-retention-privacy.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[exceptions]: app-insights-web-failures-exceptions.md
[export]: app-insights-export-telemetry.md
[exportcode]: app-insights-code-sample-export-telemetry-sql-database.md
[greenbrown]: app-insights-start-monitoring-app-health-usage.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[javareqs]: app-insights-java-track-http-requests.md
[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[older]: http://www.visualstudio.com/get-started/get-usage-data-vs
[perf]: app-insights-web-monitor-performance.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[roles]: app-insights-role-based-access-control.md
[start]: app-insights-get-started.md
[trace]: app-insights-search-diagnostic-logs.md
[track]: app-insights-custom-events-metrics-api.md
[usage]: app-insights-web-track-usage.md
[windows]: app-insights-windows-get-started.md
[windowsCrash]: app-insights-windows-crashes.md
[windowsUsage]: app-insights-windows-usage.md

