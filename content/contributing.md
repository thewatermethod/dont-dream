---
title: Contribute to Don't Dream
---

The code for this site, which includes the rules, is available on [GitHub](https://github.com/thewatermethod/dont-dream). I welcome anyone reading this to open either [issues](https://github.com/thewatermethod/dont-dream/issues) or [pull requests](https://github.com/thewatermethod/dont-dream/pulls) to improve both the functionality of the site and the rules for _Don't Dream_.

That's definitely the preferred way, but you can also use the form below to send an email if you'd like:

<style>
    button[type="submit"] {
        font-size: 1em;
    }
    form {
        margin-bottom: 1em;
    }
</style>

<form class="callout-box" netlify name="contact">
    <h2>Contact</h2>
    <div>   
        <label>
            Return Email<br />
            <input type="email" name="email" id="email" />
        </label>
    </div>
    <div>
        <label>
            Message <span class="req">*</span><br/>
            <textarea name="message" id="message" rows="5" required></textarea>
        </label>
    </div>
    <button type="submit">Send</button>
</form>
