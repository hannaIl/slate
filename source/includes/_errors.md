# Error codes

<!-- <aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside> -->

CMDB может возвращать следующие сообщения об ошибках:


Error | Meaning
-------- | -------
400 | Bad Request -- Читайте сообщение об ошибке (например: Reason: Can't find attribute {key} in {class}).
401 | Unauthorized -- Проверьте креды либо запросите доступ.
404 | Not Found -- Проверьте введенный URL.
5xx | Internal Server Error -- Обращайтесь к документации, свяжитесь с разработчиками.
