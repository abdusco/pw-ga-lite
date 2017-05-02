# MarkupGALite, ProcessWire Markup module wrapper for [ga-lite][ga-lite].

You can read the detailed [blog post][blog-post] about how I built it.

MarkupGALite is a wrapper around ga-lite, a lightweight alternative to official Google Analytics client. It only includes `pageview` event. It's cacheable, and its small print allows it to be injected / linked into page without much load.
 
The module offers several features:
- Offers a configuration page for:
    - Setting up tracking ID and IP anonymization
    - Specifying pages to inject the script
    - Choosing where to place the script (before `</head>` or `</body>`)
    - Choosing whether to link or inject complete script into HTML
- It's hookable. By hooking `MarkupGALite::renderTrackingCode`, it's possible to change the render behavior.
- Translatable. All strings are marked as translatable.

## Usage

- Install the module from **Modules** > **Install** > **Add New** using one of the provided options. Or download and extract it to `/site/modules/`, then install.
- On the configuration page, fill in the tracking code (otherwise script will not be included in page), and any other options.

You can use it manually without installing.

```php
$ga = wire()->modules->MarkupGALite;

echo $ga->renderTrackingCode([
    'trackingId' => 'UA-123XX',
    'anonymizeIp' => true
]);
```

## Hooks

You can hook into `renderTrackingCode` method.

- Change the arguments
    ```php
    wire()->addHookBefore('MarkupGALite::renderTrackingCode', function(HookEvent $e){
        $options = $e->arguments(0);
        $inject = $e->arguments(1);
    
        $options['trackingId'] = 'UA-ANOTHER';
        $inject = false;
        
        $e->setArgument(0, $options);
        $e->setArgument(1, $inject);
    });
    ```

- Disable it altogether:

    ```php
    wire()->addHookBefore('MarkupGALite::renderTrackingCode', function(HookEvent $e){
        // $someCondition = ...
        
        if ($someCondition) $e->cancelHooks = true;
    });
    ```






[ga-lite]: https://github.com/jehna/ga-lite
[blog-post]: https://abdus.co/blog/creating-a-simple-and-configurable-module-for-processwire/