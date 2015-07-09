{% if site.disqus %}
<div class="post-disqus">
    <div id="disqus_thread"></div>
    <script type="text/javascript">
    /* * * CONFIGURATION VARIABLES * * */
    var disqus_shortname = 'kalikar';
    var disqus_identifier = '{{ page.disqus.id|default('kalikar-github') }}';
    var disqus_title = '{{ page.title|default('kalikar \'s Blog') }}';
    var disqus_url = '{{ page.url | prepend: site.baseurl | prepend: site.url }}';

    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() {
     var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
     dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
     (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
     })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
  </div>
</div>
{% endif %}