# list of changes made to make this work

* Created a new spring-boot project. 
* Used wsdl2rest to create CXF and a generated camel file from the wsdl -- `http://www.dataaccess.com/webservicesserver/numberconversion.wso?WSDL`
* wsdl2rest completed successfully without errors, but we noticed a few things that had to be corrected

## corrections

1. Package names were incorrect in generated context.xml file. It is using "com/webservicesserver.dataaccess.www." in several places though "com.dataaccess.webservicesserver." is the package generated in cxf.
2. Fixing that and calling http://localhost:8081/jaxrs/numbertodollars/111 threw a fun error:

`org.apache.cxf.interceptor.Fault: class java.lang.String cannot be cast to class java.math.BigDecimal (java.lang.String and java.math.BigDecimal are in module java.base of loader 'bootstrap')
    at org.apache.cxf.jaxws.interceptors.WrapperClassOutInterceptor.handleMessage(WrapperClassOutInterceptor.java:107)
    at org.apache.cxf.phase.PhaseInterceptorChain.doIntercept(PhaseInterceptorChain.java:308)
    at org.apache.cxf.endpoint.ClientImpl.doInvoke(ClientImpl.java:537)
    at org.apache.cxf.endpoint.ClientImpl.invoke(ClientImpl.java:439)
    at org.apache.camel.component.cxf.CxfProducer.process(CxfProducer.java:133)
    at org.apache.camel.processor.SendProcessor.process(SendProcessor.java:148)
    at org.apache.camel.processor.RedeliveryErrorHandler.process(RedeliveryErrorHandler.java:548)
    at org.apache.camel.processor.CamelInternalProcessor.process(CamelInternalProcessor.java:201)
    at org.apache.camel.processor.Pipeline.process(Pipeline.java:138)
    at org.apache.camel.processor.Pipeline.process(Pipeline.java:101)
    at org.apache.camel.processor.CamelInternalProcessor.process(CamelInternalProcessor.java:201)
    at org.apache.camel.component.direct.DirectBlockingProducer.process(DirectBlockingProducer.java:53)
    at org.apache.camel.processor.SendProcessor.process(SendProcessor.java:148)
    at org.apache.camel.processor.RedeliveryErrorHandler.process(RedeliveryErrorHandler.java:548)
    at org.apache.camel.processor.CamelInternalProcessor.process(CamelInternalProcessor.java:201)
    at org.apache.camel.processor.CamelInternalProcessor.process(CamelInternalProcessor.java:201)
    at org.apache.camel.component.jetty.CamelContinuationServlet.doService(CamelContinuationServlet.java:215)
    at org.apache.camel.http.common.CamelServlet.service(CamelServlet.java:78)
    at javax.servlet.http.HttpServlet.service(HttpServlet.java:750)
    at org.eclipse.jetty.servlet.ServletHolder.handle(ServletHolder.java:865)
    at org.eclipse.jetty.servlet.ServletHandler.doHandle(ServletHandler.java:535)
    at org.eclipse.jetty.server.handler.ScopedHandler.nextHandle(ScopedHandler.java:255)
    at org.eclipse.jetty.server.handler.ContextHandler.doHandle(ContextHandler.java:1340)
    at org.eclipse.jetty.server.handler.ScopedHandler.nextScope(ScopedHandler.java:203)
    at org.eclipse.jetty.servlet.ServletHandler.doScope(ServletHandler.java:473)
    at org.eclipse.jetty.server.handler.ScopedHandler.nextScope(ScopedHandler.java:201)
    at org.eclipse.jetty.server.handler.ContextHandler.doScope(ContextHandler.java:1242)
    at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:144)
    at org.eclipse.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:132)
    at org.eclipse.jetty.server.Server.handle(Server.java:503)
    at org.eclipse.jetty.server.HttpChannel.handle(HttpChannel.java:364)
    at org.eclipse.jetty.server.HttpConnection.onFillable(HttpConnection.java:260)
    at org.eclipse.jetty.io.AbstractConnection$ReadCallback.succeeded(AbstractConnection.java:305)
    at org.eclipse.jetty.io.FillInterest.fillable(FillInterest.java:103)
    at org.eclipse.jetty.io.ChannelEndPoint$2.run(ChannelEndPoint.java:118)
    at org.eclipse.jetty.util.thread.QueuedThreadPool.runJob(QueuedThreadPool.java:765)
    at org.eclipse.jetty.util.thread.QueuedThreadPool$2.run(QueuedThreadPool.java:683)
    at java.base/java.lang.Thread.run(Thread.java:834)
Caused by: java.lang.ClassCastException: class java.lang.String cannot be cast to class java.math.BigDecimal (java.lang.String and java.math.BigDecimal are in module java.base of loader 'bootstrap')
    at com.dataaccess.webservicesserver.NumberToDollars_WrapperTypeHelper1.createWrapperObject(Unknown Source)
    at org.apache.cxf.jaxws.interceptors.WrapperClassOutInterceptor.handleMessage(WrapperClassOutInterceptor.java:91)
    ... 37 more`
