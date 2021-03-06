<h1 data-book="configuration">Configuration Reference</h1>

This chapter documents each Rythm configuration item details. For how to configure Rythm engine and configuration rules, please refer to the [developer guide](developer_guide.md#configuration)

### [engine]Engine configuration

This section list the core engine configurations including:

* [engine.class_loader.byte_code_helper.impl](#engine_class_loader_byte_code_helper_impl)
* [engine.class_loader.parent.impl](#engine_class_loader_parent_impl)
* [engine.file_write.enabled](#engine_file_write_enabled)
* [engine.load_precompiled.enabled](#engine_load_precompiled_enabled)
* [engine.mode](#engine_mode)
* [engine.plugin.version](#engine_plugin_version)
* [engine.precompile_mode.enabled](#engine_precompile_mode_enabled)

#### [engine_class_loader_byte_code_helper_impl]engine.class_loader.byte_code_helper.impl

Aliases: 

* **engine.class_loader.bytecode_helper**
* **rythm.engine.class_loader.bytecode_helper**
* **rythm.engine.class_loader.bytecode_helper.impl**

Inject a [bytecode helper](http://rythmengine.org/api/org/rythmengine/extension/IByteCodeHelper.html) implementation into Rythm in memory compilation system to allow user application specified way to locate class bytecode.

Default value: `null`

See also: [org.rythmengine.extension.IByteCodeHelper](http://rythmengine.org/api/org/rythmengine/extension/IByteCodeHelper.html)

#### [engine_class_loader_parent_helper_impl]engine.class_loader.parent.impl

Aliases:

* **engine.class_loader.parent**
* **rythm.engine.class_loader.parent**
* **rythm.engine.class_loader.parent.impl**

Set the parent class loader to the Rythm template class loader(`org.rythmengine.internal.compiler.TemplateClassLoader`). In certain environment, for example, Play!framework, a specific class loader is used to manager application classes. Thus it needs to set the parent of the tempmlate class loader so that when a application class is encountered in the template source, the correct class can be located.

Default value: `Thread.currentThread().getContextClassLoader()`

#### [engine_file_write_enabled]engine.file_write.enabled

Aliases:

* **engine.file_write**
* **rythm.engine.file_write**
* **rythm.engine.file_write.enabled**

Enable engine to write to disk to output the generated source file and cache bytecode files. By default file write is enabled. However in certain environment, for example, Google Applicaiton Engine, file writing is forbidden. In which case you want to disable file write of the Rythm engine

Default value: `true`

#### [engine_id]engine.id

Aliases:

* **rythm.engine.id**

Set the ID to the engine instance. The ID been set could be later on retreived via [RythmEngine.id()](http://rythmengine.org/api/org/rythmengine/RythmEngine.html#id()) instance method call. This is useful when a user application instantiated multiple rythm engines, and need to distinct one from the others.

Default value: a `re-` prefix with another three random characters

#### [engine_load_precompiled_enabled]engine.load_precompiled.enabled

Aliases:

* **engine.load_precompiled**
* **rythm.engine.load_precompiled**
* **rythm.engine.load_precompiled.enabled**

Use this configuration to instruct the engine to load precompiled template bytecode directly instead of parsing and compiling the template source. Usually used in product mode when bytecode has already been [precompiled](#precompile_mode)

Default value: `false`

See also: [engine.precompile.mode](#precompile_mode)

#### [engine_mode]engine.mode

Aliases:

* **rythm.engine.mode**

Set the engine running mode. The Rythm engine could be running in either [dev](http://rythmengine.org/api/org/rythmengine/Rythm.Mode.html#dev) or [prod](http://rythmengine.org/api/org/rythmengine/Rythm.Mode.html#prod) mode. 

When engine is running in dev mode, each time any form of render method is called, the engine will check if the template source file has changed since last parsing/compilation or not. If the file has been changed, then it will reload the template source file and go through the parsing/compilation process to generate new bytecodes. Otherwise, the engine will just continue to execute the existing bytecodes. 

Another thing about dev mode is, when the [engine.file_write](#engine_file_write) is enabled, then the generated bytecodes will be persistent to [home.tmp](#home_tmp) directory so that they can be quickly loaded when next time the program started.

Default value: `prod`

#### [engine_plugin_version]engine.plugin.version

Aliases:

* **rythm.engine.plugin.version**

Set by a plugin which built on top of Rythm engine, e.g. PlayRythm plugin which provide Rythm template engine to PlayFramework. Once set, the version number will be checked along with Rythm engine's [version](http://rythmengine.org/api/org/rythmengine/RythmEngine.html#version()) number against the version info written into the previously generated bytecode cache files to see if template source needs to be re-processed or not

Default value: empty String `""`

#### [engine_precompile_mode_enabled]engine.precompile_mode.enabled

Aliases

* **engine.precompile_mode**
* **rythm.engine.precompile_mode**
* **rythm.engine.precompile_mode.enabled**

When engine is run in precompile mode the generated template bytecodes will be cached to [precompiled](#home_precompiled) directory, otherwise they will be cached to the [tmp](#home_tmp) directory. The cached bytecode files can be loaded by engine when [engine.load_precompiled](#engine_load_precompiled) set to `true`

Default value: `false`

See also: 
- [engine.load_precompiled](#engine_load_precompiled)
- [home.precompiled](#home_precompiled)

### [feature]Feature enable/disable

This section describe configurations that turn on/off Rythm features including:

* [feature.natural_template.enabled](#feature_natural_template_enabled)
* [feature.smart_escape.enabled](#feature_smart_escape_enabled)
* [feature.transform.enabled](#feature_transform_enabled)
* [feature.type_inference.enabled](#feature_type_inference_enabled)
* [feature.dynamic_exp.enabled](#feature_dynamic_exp_enabled)

#### [feature_natural_template_enabled]feature.natural_template.enabled

Aliases

* **feature.natural_template**
* **rythm.feature.natural_template**
* **rythm.feature.natural_template.enabled**

As per [wikipedia](http://en.wikipedia.org/wiki/Template_engine_(web)), a natural template means *the template can be a document as valid as the final result, the engine syntax doesn't break the document's structure*

When natural template feature is enabled, Rythm engine support put directives into comments of the context code type, thus it won't break the document's structure:

```lang-java,fid-0edb44d950eb4e9ba4a9355041e3a8b9
<!-- @args String who -->
<p>Hello @who.capFirst()!</p>
----------------------------------
<!-- @if (true) { -->
<p>true</p>
<!-- } -->
----------------------------------
<script>

/* @for ([1..5]) { */
alert(@_);
/* } */

</script>
```

<div class="alert alert-info"><b>Note</b>: turning on natural template feature might increase the template parsing time.</div>

Default value: `false`

#### [feature_smart_escape_enabled]feature.smart_escape.enabled

Aliases

* **feature.smart_escape**
* **rythm.feature.smart_escape**
* **rythm.feature.smart_escape.enabled**

When `smart_escape` is turned on Rythm automatically [escape](expression.md#escape) the expression output in a way that corresponds the the code type context. The following code shows how Rythm escape variables in a different way when it inside `html` context and `javascript` context respectively:

```java-lang,fid-7dc65a5ff7e84aeeb7bd3052cd5d152f
@args String html, String js
<p>
  html: @html
  js: @js
</p>
<script>
    html: @html
    js: @js
</script>
```

<div class="alert alert-info"><b>Note</b>: turning on smart escape feature might increase the template parsing time.</div>

Default value: `true`

#### [feature_transform_enabled]feature.transform.enabled

Aliases

* **feature.transform**
* **rythm.feature.transform**
* **rythm.feature.transform.enabled**

When transform feature is turned on, you can use Rythm [built-in](builtin_transformer.md) transformers and [user defined transformers](user_defined_transformer.md) in your template to transform expression output. In the following code, the built-in transformer `format` is used to transform the `theDate` variable output:

```lang-java,fid-35de9612490d4415a72117414a94de63
@args Date theDate
@theDate
@theDate.format()
@theDate.format("yyyy-MM-dd")
@theDate.format("dd/MMM/yyyy")
```

<div class="alert alert-info"><b>Note</b>: turning on smart escape feature might increase the template parsing time.</div>

Default value: `true`

#### [feature_type_inference_enabled]feature.type_inference.enabled

Aliases:

* **feature.type_inference**
* **rythm.feature.type_inference**
* **rythm.feature.type_inference.enabled**

Being a static typed template engine, Rythm require [type declaration](directive.md#args) of template arguments. When `feature.type_inference` is enabled, Rythm will infer the argument types using the parameters passing to the template at the first time. Meaning you don't need to use the `@args` directive to declare template arguments. However the feature is very restricted in usage:

1. The `type_inference` feature does not work with `engine.precompile_mode`. The reason is simple, there is no parameter when you try to precompile the template, thus type inference is not possible
2. If a parameter is passed in as `null`, then the type of the corresponding argument will be inferred as `java.lang.Object`

<div class="alert alert-warn"><b>Warn</b>, use <code>Type Inference</code> feature with caution, please sure you fully understand the above restrictions of this feature before turning it on</div>

Default value: `false`

### [home]Home directories

This section describe the directories configurations used in Rythm including:

* [home.template.dir](#home_template_dir)
* [home.tmp.dir](#home_tmp_dir)
* [home.precompiled.dir](#home_precompiled_dir)


#### [home_template_dir]home.template.dir

Aliases:

* **home.template**
* **rythm.home.template**
* **rythm.home.template.dir**

Set the root directory of template source files. This configuration is used by Rythm to locate template source files if you have not configured the [resource loader implementation](#resource_loader) or your configured resource loader failed to load the template specified.

Default value: The first `rythm` folder found in the class root of current Java process:

```lang-java
new File(
    Thread.currentThread()
          .getContextClassLoader()
          .getResource("rythm")
          .getFile())
```

#### [feature_dynamic_exp_enabled]feature.dynamic_exp.enabled

If this configuration is set to true, then Rythm will generate code to support Javabeans notation in template source code.

Without dynamic expression enabled, you have to write

```lang-java
@user.getName()
@map.get("key")
```

With dynamic expression enabled, you are able to write

```lang-java
@user.name
@map.key
```

<div class="alert alert-warn">
<b>Warn</b> there are about 20% ~ 30% performance downgrade with dynamic expression enabled. However the performance will still beat velocity and freemarker anyway ;-)
</div>

#### [home_tmp_dir]home.tmp.dir

Aliases:

* **home.tmp**
* **rythm.home.tmp**
* **rythm.home.tmp.dir**

Set the root directory to output the generated template java code and byte code files. This is very helpful when running Rythm engine in dev mode, as developer can set the tmp dir as a source root folder in his/her IDE, and set breakpoint to the generated java code files to debug the template. The byte code been cached is also useful as Rythm engine will try to load the cached bytecodes directly if the template source timestamp is not changed, and thus safe the template parsing and compilation time when running in dev mode.

Default value: The `__rythm` folder under the system tmp directory: 

```lang-java
new File(System.getProperty("java.io.tmpdir"), "__rythm")
```

#### [home_precompiled_dir]home.precompiled.dir

Aliases:

* **home.precompiled**
* **rythm.home.precompiled**
* **rythm.home.precompiled.dir**

Set the root directory to output the generated template byte code files when the engine is running under [precompile mode](#engine_precompile_mode). The generated template byte codes can be loaded in later process when the [load_precompiled](#engine_load_precompiled) option is turned on

Default value: `null`

See also

* [precompile_mode option](#engine_precompile_mode)
* [load_precompiled option](#engine_load_precompiled)

### [resource]Resource loading settings

This section documents settings pertinant to resource (template source) loading which includes:

* [resource.loader.impl](#resource_loader_impl)
* [resource.name.suffix](#resource_name_suffix)

#### [resource_loader_impl]resource.loader.impl

Aliases:

* **resource.loader**
* **rythm.resource.loader**
* **rythm.resource.loader.impl**

Configure [resource loader](http://rythmengine.org/api/org/rythmengine/extension/ITemplateResourceLoader.html) implementation. When resource loader is configured, then the engine will ask it to load template source when needed.

Default value: `null`

#### [resource_name_suffix]resource.name.suffix

Aliases:

* **rythm.resource.name.suffix**

Set suffix (e.g. "`.rythm`") of the template source files. Note this setting is favor by Rythm's built-in file resource loader. However when the [resource loader](#resource_loader) is configured, it is up to the user implementation to decide whether favor this setting or not.

Default value: empty string `""` 

When you use the name suffix you can still have the regular file extensions. For example, suppose your name suffix is `.rythm`, the following names are all valid rythm template soure file names:

* `foo.html.rythm`
* `foo.js.rythm`
* `foo.rythm`

The first two file names are more preferred as they keep the file type info in the `.html` and `.js` extension. Rythm engine can use them to deduct the [code type](#default_code_type_impl) of the template.

<div class="alert"><b>Warn</b> Do not use regular file extensions for this setting, e.g. <code>.html</code>, <code>.js</code>, <code>.json</code> etc. As they could be used to identify the <a href="#default_code_type_impl">code type</a> of the template</div>

### [codegen]Code generation options

This section describe configurations impact code generation including:

* [codegen.byte_code_enhancer.impl](#codegen_byte_code_enhancer_impl)
* [codegen.compact.enabled](#codegen_compact_enabled)
* [codegen.source_code_enhancer.impl](#codegen_source_code_enhancer_impl)

#### [codegen_byte_code_enhancer_impl]codegen.byte_code_enhancer.impl

Aliases

* **codegen.byte_code_enhancer**
* **rythm.codegen.byte_code_enhancer**
* **rythm.codegen.byte_code_enhancer.impl**

Use this configuration to setup [byte code enhancer](http://rythmengine.org/api/org/rythmengine/extension/IByteCodeEnhancer.html) which further process the byte code generated out of the template source. For example the [PlayRythm plugin](http://playframework.org/modules/rythm) use this configuration to apply play's property enhancement on the template class byte code.

Default value: `null`

See also: [org.rythmengine.extension.IByteCodeEnhancer](http://rythmengine.org/api/org/rythmengine/extension/IByteCodeEnhancer.html)

#### [codegen_compact_enabled]codegen.compact.enabled

Aliases:

* **codegen.compact**
* **rythm.codegen.compact**
* **rythm.codegen.compact.enabled**

When this option is turned on Rythm will compact template output by removing extra white space and line breaks.

Default value: `true`

#### [codegen_source_code_enhancer_impl]codegen.source_code_enhancer.impl

Aliases:

* **codegen.source_code_enhancer**
* **rythm.codegen.source_code_enhancer**
* **rythm.codegen.source_code_enhancer.impl**

Set the [source code enhancer](http://rythmengine.org/api/org/rythmengine/extension/ISourceCodeEnhancer.html) implementation to Rythm code generation system to allow user application inject imports, source codes and template arguments to the generated java source file. 

Default value: `null`

### [render]Render options

This section documents options used at template rendering time including:

* [render.listener.impl](#render_listener_impl)
* [render.exception_handler.impl](#render_exception_handler_impl)

#### [render_listener_impl]render.listener.impl

Aliases:

* **render.listner**
* **rythm.render.listner**
* **rythm.render.listner.impl**

Configure the [render listener](http://rythmengine.org/api/org/rythmengine/extension/IRythmListener.html). The listener allows user plugin application specific logics on different rendering events, including render, post render, invoke, post invoke.

Default value: `null`

#### [render_exception_handler_impl]render.exception_handler.impl

Aliases:

* **render.exception_handler**
* **rythm.render.exception_handler**
* **rythm.render.exception_handler.impl**

Configure the [render exception handler](http://rythmengine.org/api/org/rythmengine/extension/IRenderExceptionHandler.html). The handler method is invoked when exception encountered while executing a template. For example, PlayRythm plugin use this handler to process play's specific `Render` exception so that template author can call controller action directly from template.

Default value: `null`

### [built_in]Built-ins

This section documents options to turn on/off built-in feature implementations including:

* [built_in.code_type.enabled](#built_in_code_type_enabled)
* [built_in.transformer.enabled](#built_in_transformer_enabled)

#### [built_in_code_type_enabled]built_in.code_type.enabled

Aliases

* **built_in.code_type**
* **rythm.built_in.code_type**
* **rythm.built_in.code_type.enabled**

Enable [built-in code type](http://rythmengine.org/api/org/rythmengine/extension/ICodeType.DefImpl.html) implementation. The built-in code type implements common code types including html, xml, javascript, css, csv and JSON. Usually you should not turn off this option.

Default value: `true` 

#### [built_in_transformer_enabled]built_in.transformer.enabled

Aliases:

* **built_in.transformer**
* **rythm.built_in.transformer**
* **rythm.built_in.transformer.enabled**

Enable [built-in transformers](builtin_transformer.md). The built-in transformers provides commonly used utilities including different escape schemes, String manipulation, date and currency format, i18n and list join etc.

Default value: `true`

### [default]Defaults

This section documents default settings including:

* [default.code_type.impl](#default_code_type_impl)
* [default.cache_ttl](#default_cache_ttl)

#### [default_code_type_impl]default.code_type.impl

Aliases:

* **default.code_type**
* **rythm.default.code_type**
* **rythm.default.code_type.impl**

Specify the default [code type](http://rythmengine.org/api/org/rythmengine/extension/ICodeType.html). You can set this configuration when you are using Rythm in a specific senario. For example:

* Case one, use Rythm to generate html pages in web project, set default code type to
    * either ["org.rythmengine.extension.ICodeType.DefaultImpl.HTML"](http://rythmengine.org/api/org/rythmengine/extension/ICodeType.DefImpl.html#HTML) 
    * or ["org.rythmengine.extension.ICodeType.DefaultImpl.XML"](http://rythmengine.org/api/org/rythmengine/extension/ICodeType.DefImpl.html#XML)
* Case two, use Rythm to generate csv files, set default code type to
    * ["org.rythmengine.extension.ICodeType.DefaultImpl.CSV"](http://rythmengine.org/api/org/rythmengine/extension/ICodeType.DefImpl.html#CSV)

Default value: [org.rythmengine.extension.ICodeType.DefaultImpl.RAW](http://rythmengine.org/api/org/rythmengine/extension/ICodeType.DefImpl.html#RAW)

#### [default_cache_ttl]default.cache_ttl

Aliases:

* **rythm.default.cache_ttl**

Specify the default timeout in second for Rythm's cache service. This option is effective only when [cache](#cache) is enabled and [cache service]() is not set to user implementation

Default value: `60 * 60` (1 hour)

### [cache]Cache settings

This section documents cache related settings including:

* [cache.enabled](#cache_enabled)
* [cache.service.impl](#cache_service_impl)
* [cache.duration_parser.impl](#cache_duration_parser_impl)
* [cache.prod_only.enabled](#cache_prod_only_enabled)

#### [cache_enabled]cache.enabled

Aliases:

* **cache**
* **rythm.cache**
* **rythm.cache.enabled**

Enable/disable cache service. When cache service is enabled, template author can use [@cache](directive.md#cache) to direct engine to cache certain part of rendered content.

Default value: `false`

#### [cache_service_impl]cache.service.impl

Aliases

* **cache.service**
* **rythm.cache.service**
* **rythm.cache.service.impl**

Set the [cache service](http://rythmengine.org/api/org/rythmengine/extension/ICacheService.html) implementation. This configuration is only effective when [cache](#cache_cache) is enabled.

Default value: Rythm's built-in [simple cache service](http://rythmengine.org/api/org/rythmengine/cache/SimpleCacheService.html)

#### [cache_duration_parser_impl]cache.duration_parser.impl

Aliases

* **cache.duration_parser**
* **rythm.cache.duration_parser**
* **rythm.cache.duration_parser.impl**

Set the [duration parser](http://rythmengine.org/api/org/rythmengine/extension/IDurationParser.html) implementation. This configuration is only effective when [cache](#cache_cache) is enabled.

Default value: Rythm's built-in [duration parser](http://rythmengine.org/api/org/rythmengine/extension/IDurationParser.html#DEFAULT_PARSER)

#### [cache_prod_only_enabled]cache.prod_only.enabled

Aliases

* **cache.prod_only**
* **rythm.cache.prod_only**
* **rythm.cache.prod_only.enabled**

When this configuration is set to `true`, then cache will only be effective when Rythm is running in [prod](http://rythmengine.org/api/org/rythmengine/Rythm.Mode.html#prod) mode. Disable cache at [dev](http://rythmengine.org/api/org/rythmengine/Rythm.Mode.html#dev) mode makes it convenient for development and debugging.

Default value: `true`

### [log]Log settings

This section documents log settings including:

* [log.enabled](#log_impl)
* [log.factory.impl](#log_factory_enabled)
* [log.source.java.enabled](#log_source_java_enabled)
* [log.source.template.enabled](#log_source_template_enabled)
* [log.time.render.enabled](#log_time_render_enabled)

#### [log_enabled]log.enabled

Aliases:

* **log**
* **rythm.log**
* **rythm.log.enabled**

Enable/disable logging in Rythm.

Default value: `true`

#### [log_factory_impl]log.factory.impl

Aliases:

* **log.factory**
* **rythm.log.factory**
* **rythm.log.factory.impl**

Configure [log factory](http://rythmengine.org/api/org/rythmengine/extension/ILoggerFactory.html) implementation.

Default value: the rythm built-in [org.rythmengine.logger.JDKLogger.Factory](http://rythmengine.org/api/org/rythmengine/logger/JDKLogger.Factory.html)

#### [log_source_java_enabled]log.source.java.enabled

Aliases:

* **log.source.java**
* **rythm.source.java**
* **rythm.source.java.enabled**

Enable/disable print relevant section of generated java source code when exception encountered.

Default value: `true`

#### [log_source_template_enabled]log.source.template.enabled

Aliases:

* **log.source.template**
* **rythm.source.template**
* **rythm.source.template.enabled**

Enable/disable print relevant section of template source code when exception encountered.

Default value: `true`

#### [log_time_render_enabled]log.time.render.enabled

Aliases:

* **log.time.render**
* **rythm.time.render**
* **rythm.time.render.enabled**

Enable/disable log template render time using [DEBUG](http://rythmengine.org/api/org/rythmengine/logger/ILogger.html#debug(java.lang.String, java.lang.Object...)) level

Default value: `false`


### [i18n]I18N settings

This section documents i18n relevant configurations including:

* [i18n.locale](#i18n_locale)
* [i18n.message.sources](#i18n_message_sources)
* [i18n.message.resolver.impl](#i18n_message_resolver_impl)

#### [i18n_locale]i18n.locale

Aliases:

* **rythm.i18n.locale**

Set the locale which impact the output of [@i18n](directive.md#i18n) built-in function, varieties of [format](builtin_transformer.md#format) built-in transformers etc. 

The locale set with this configuration can be overwritten by [@locale](directive.md#locale) directive when executing the template.

Default value: `java.util.Locale.getDefault()`

Set `i18n.locale` by API:

```lang-java
Map<String, Object> conf = ...
Locale locale = new Locale("en", "AU");
conf.put("i18n.locale", locale);
// or
conf.put("i18n.locale", "cn_ZH");
...
```

Set `i18n.locale` in properties file:

```
rythm.i18n.locale=cn_ZH
# rythm.i18n.locale=en
```

#### [i18n_message_sources]i18n.message.sources

Aliases:

* **rythm.i18n.message.sources**

Set message sources, should be a String separated by "`,`", for example, `"format,exception,window"`. Note the setting is used by Rythm's default [i18n.message.resolver](#i18n_message_resolver) implementation. If user application configure the customized message resolver, it is up to that message resolver implementation to decide whether use this configuration or not.

Default value: `"messages"`

#### [i18n_message_resolver_impl]i18n.message.resolver.impl

Aliases:

* **i18n.message.resolver**
* **rythm.i18n.message.resolver**
* **rythm.i18n.message.resolver.impl**

Configure [i18n message resolver](http://rythmengine.org/api/org/rythmengine/extension/II18nMessageResolver.html) which support i18n message lookup.

Default value: <a href="http://rythmengine.org/api/org/rythmengine/extension/II18nMessageResolver.DefaultImpl.html"><code>org.rythmengine.extension.II18nMessageResolver.DefaultImpl</code></a>

### [sandbox]Sandbox settings

This section documents [sandbox](sandbox.md) relevant configurations including:

* [sandbox.security_manager.impl](#sandbox_security_manager_impl)
* [sandbox.timeout](#sandbox_timeout)
* [sandbox.restricted_class](#sandbox_restricted_class)
* [sandbox.allowed_system_properties](#sandbox_allowed_system_properties)
* [sandbox.thread_factory.impl](#sandbox_thread_factory_impl)
* [sandbox.pool.size](#sandbox_pool_size)

To understand sandbox concept and check how to use sandbox, please click [here](sandbox.md).

#### [sandbox_security_manager_impl]sandbox.security_manager.impl

Aliases:

* **sandbox.security_manager**
* **rythm.sandbox.security_manager**
* **rythm.sandbox.security_manager.impl**

Set the security manager for sandbox rendering. A security manager guard the running system from being breaking by mal codes in the template source. For example, `@{Runtime.exit(0);}`

Default value: `org.rythmengine.sandbox.RythmSecurityManager`

#### [sandbox_timeout]sandbox.timeout

Aliases:

* **rythm.sandbox.timeout**

Set timeout in milliseconds for sandbox rendering thread. The timeout setting gives a chance for the system to exit a template executing if it takes too much time.

Default value: `2000` (2 seconds)

#### [sandbox_restricted_class]sandbox.restricted_class

Aliases:

* **rythm.sandbox.restricted_class**

Configure the classes or packages that cannot be referenced in a template when running in sandbox mode. The configuration should be a String contains class/package names separated by "`,`".

Default value: empty string `""` 

<div class="alert">
<b>Note</b>: even if application has not specify any restricted class names, Rythm will still prevent the following classes in the template source when run it in sandbox mode:
<ul>
<li>org.rythmengine.Rythm</li>
<li>org.rythmengine.RythmEngine</li>
<li>java.io</li>
<li>java.nio</li>
<li>java.security</li>
<li>java.rmi</li>
<li>java.net</li>
<li>java.awt</li>
<li>java.applet</li>
</ul>
</div>

#### [sandbox_allowed_system_properties]sandbox.allowed_system_properties

Aliases:

* **rythm.sandbox.allowed_system_properties**

Specify the system properties in a string separated by `,` that can be accessed from the template source code when running in sandbox mode. If the template source in anyway accessing `java.lang.System.properties` for properties not specified in this configuration then Rythm will throw out an exception.

Default value is a string composed by the following items separated by `,`:

* java.io.tmpdir
* file.encoding
* user.dir
* line.separator
* java.vm.name
* java.protocol.handler.pkgs
* suppressRawWhenUnchecked

<div class="alert"><i class="icon-warning-sign"></i> if you set this configuration, make sure your setting contains the above properties, otherwise you might have trouble to run a template in sandbox mode
</div>


#### [sandbox_thread_factory_impl]sandbox.thread_factory.impl

Aliases:

* **sandbox.thread_factory**
* **rythm.sandbox.thread_factory**
* **rythm.sandbox.thread_factory.impl**

Set the thread factory that creates template executing thread when running in sandbox mode. When this setting is not configured by user application, Rythm will handle the sandbox executing thread management

Default value: `null`

#### [sandbox_pool_size]sandbox.pool.size

Aliases:

* **rythm.sandbox.pool.size**

Set the maximum number of sandbox executing threads in the thread pool. 

<div class="alert"><b>Note</b>: if user has set the <a href="#sandbox_thread_factory_impl">thread factory</a> implementation, it is up to the custom thread factory implementation to decide whether favor the <code>sandbox.pool.size</code> setting or not</div>

Default value: 10

### [ext]Extensions

This section documents developer extension configurations 

* [ext.transformer](#ext_transformer)
* [ext.prop_accessor](#ext_prop_accessor)

Aliases:

* **rythm.transformer.udt**

#### [ext_transformer]User defined transformers

Register [user defined transformers](user_defined_transfomer.md). This configuration support different type of parameter:

* An array of class name or `Class` instance
* A list of class name or `Class` instance
* A `Class` instance
* A string of class names separated by `,` or space

Except the last type, all other types can only be used configured with API call. The last one can be used by both via API or via properties file.

Configure `transformer.udt` via API call:

```lang-java
Map<String, Object> conf = ...
// configure it with array of classes
Class[] udts = new Class[]{MyTrans1.class, MyTrans2.class, ...};
conf.put("transformer.udt", udts);
// configure it with list of classes
List<Class> lc = ...
lc.add(MyTrans1.class);
lc.add(MyTrans2.class);
// configure it with one class instance
conf.put("transfomer.udt", MyTrans1.class);
// configure it with string
conf.put("transformer.udt", "MyTrans1.class, MyTrans2.class");
```

Configure `transformer.udt` via properties file or system properties:

```
transfomer.udt=MyTrans1.class,MyTrans2.class,...
```
