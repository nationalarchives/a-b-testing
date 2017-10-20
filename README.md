# A/B Testing Experiments

## Purpose

Provides simple sandbox for experimenting with A/B Testing.

## What's being demonstrated

### Google Optimize A/B Test

There is an active Google Optimize experiment on the site at [http://a-b-testing-experiments.azurewebsites.net/](http://a-b-testing-experiments.azurewebsites.net/) which shows:

* 50% of visitors teal buttons
* 50% of visitors black buttons with a teal border 

These changes were made in the Google Optimize dashboard by creating a variant and assigning the related CSS styles. It requires an amendment to the Google Analytics code snippet as follows: 

```html
<style>.async-hide { opacity: 0 !important} </style>
<script>(function(a,s,y,n,c,h,i,d,e){s.className+=' '+y;h.start=1*new Date;
        h.end=i=function(){s.className=s.className.replace(RegExp(' ?'+y),'')};
        (a[n]=a[n]||[]).hide=h;setTimeout(function(){i();h.end=null},c);h.timeout=c;
    })(window,document.documentElement,'async-hide','dataLayer',4000,
        {'GTM-WK69MVV':true});</script>

<script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
        (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
        m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
    ga('create', 'UA-108354863-1', 'auto');
    ga('require', 'GTM-WK69MVV');
    ga('send', 'pageview');
</script>
```

Having looked at the behaviour of this code it seems Google achieve this by:

* applying the `.async-hide` class to prevent a flash of unstyled content as the page loads
* injecting inline `<style>` blocks with the rules added via the dashboard
* removing the `.async-hide` class to reveal the content, which now has the desired styles applied. 

Note: while this does not seem to significantly impact upon progressive enhancement since those users who are unable to run JavaScript will see the original variant, those users who do have JavaScript will have any content with `.async-hide` hidden from them until the relevant scripts have run. 

## Development

### Development environment

Simply clone this repository to your development machine and start a PHP development server with `php -S localhost:8000`.

### Cloud deployment

There is an Azure website at [http://a-b-testing-experiments.azurewebsites.net/](http://a-b-testing-experiments.azurewebsites.net/) tracking `master` on GitHub.