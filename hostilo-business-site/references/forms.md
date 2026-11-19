# Contact forms

Hostilo sites have a built-in form endpoint: no server code, messages go to the site owner's e-mail and to the Hostilo dashboard (CSV export and webhook on PRO and Business).

## Minimal form (works without JavaScript)

```html
<form action="https://hostilo.app/api_form.php" method="POST" class="contact-form">
  <input type="hidden" name="site_token" value="SITE_TOKEN">
  <input type="hidden" name="_next" value="https://example.com/dziekujemy">
  <!-- anti-spam trap: keep hidden, never fill -->
  <input type="text" name="_honey" tabindex="-1" autocomplete="off" style="display:none">

  <label for="f-name">Imię i nazwisko</label>
  <input id="f-name" name="name" autocomplete="name">

  <label for="f-email">E-mail *</label>
  <input id="f-email" name="email" type="email" required autocomplete="email">

  <label for="f-company">Firma</label>
  <input id="f-company" name="company" autocomplete="organization">

  <label for="f-msg">Wiadomość *</label>
  <textarea id="f-msg" name="message" required maxlength="5000"></textarea>

  <label><input type="checkbox" name="consent" value="yes" required>
    Zgadzam się na kontakt w sprawie zapytania. <a href="/polityka-prywatnosci">Polityka prywatności</a></label>

  <button type="submit">Wyślij</button>
</form>
```

## Fields

| Field | Rule |
|---|---|
| `site_token` | required, hidden. The site's own token: shown by `get_site` ("Contact form" section) and in the Hostilo dashboard (site Settings → form). The same token for every language. It is public by design. |
| `email` | required, valid e-mail. Replies go to this address. |
| `message` | required, up to 5000 characters. |
| `name` | optional. |
| other fields | up to 20 extra fields, names without a leading `_`, up to 50 characters, values up to 1000 characters (e.g. `company`, `phone`, `product`, `consent`). |
| `_honey` | anti-spam trap, must stay empty and hidden. |
| `_next` | optional thank-you page: full `https://` URL **on the same site** (subdomain or connected custom domain). One per language (`/dziekujemy`, `/en/thank-you`). Without it Hostilo shows its own thank-you page in the visitor's browser language. |

## Rules

- **Never** `mailto:` forms, never third-party form services when the Hostilo form is enough.
- The endpoint accepts posts only from the site's own addresses (Origin check). Test on the live site, not from a local file.
- Label every field (`<label for>`), mark required fields, use `autocomplete`, show errors in the page language.
- Consent checkbox with a link to the privacy policy for every form that collects personal data (EU). Do not pre-tick it. Marketing consent is a separate, optional checkbox.
- Limits: Free 50 messages a month per account; PRO and Business unlimited; 10 messages per hour per visitor IP.
- Each language version has its own form page with translated labels and its own `_next`.

## With JavaScript (no page reload)

```js
form.addEventListener('submit', async (e) => {
  e.preventDefault();
  const r = await fetch(form.action, { method: 'POST', body: new FormData(form), headers: { Accept: 'application/json' } });
  const j = await r.json(); // { status: 'success' | 'error', message }
  // show j.status === 'success' ? thank-you text : error text (in the page language, not j.message)
});
```

## CRM and other forms

Embedded forms from a CRM (HubSpot, Pipedrive, Brevo…) work: external scripts are allowed on Hostilo sites. Record the form id and where it is used in `SITE.md`. If the customer wants messages in their CRM but a simple form on the site, use the Hostilo form with the webhook (PRO/Business) to Make, n8n or Zapier.
