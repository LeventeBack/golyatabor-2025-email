# Gólyatábor invitation email

This is the HTML invitation email for the Matek-Infó Gólyatábor, sent by BBTE, the Farkas Gyula Egyesület and A Kocka Másik Oldala.

- `index.html` is the current email. Earlier versions are kept in `index-2024.html` and `index-orig.html`.
- `images/` holds the source images. `psd/` holds their Photoshop files.
- `links.txt` lists the hosted image URLs.

## 1. Update the email for a new year

All the text and links are in `index.html`. Search for each value below and replace it. The form link appears twice, so change both.

| What | Search for | Notes |
| --- | --- | --- |
| Title | `Gólyatábor 2026` | Top of the main text |
| Dates | `Dátum:` | e.g. `2026. október 23–25.` |
| Location | `Helyszín` | Change both the address text and the Google Maps link (`maps.app.goo.gl/...`) |
| Price | `Ár` | Total price |
| Getting there | `Utazás` | Bus line and directions |
| Rules document | `docs.google.com` | Link to this year's signed-rules document |
| Application deadline | `Jelentkezési határidő` | |
| Application form | `forms.gle` | **Appears twice:** the **JELENTKEZZ ITT** button near the top, and the `Jelentkezés:` line |
| Deposit and remainder | `100 lej előleget`, `200 lej` | Keep them consistent with the total price |
| Revolut account | `revolut.me` | Change both the link and the `@name` shown |
| Story text | `A sátor áll` | The themed intro paragraphs, rewritten each year |
| Banner | `bannerkesz` | The `src` of the banner image. Upload the new banner first (see section 2) |

Then open `index.html` in a browser and check it at phone width as well. In Chrome, open DevTools and turn on the device toolbar.

## 2. Host the images

Email apps can't load images from your computer, so every image must have a public URL. We host them on **Cloudinary**: upload each file, copy its URL, and paste it into `index.html`.

| Image | Used for | Colour | Source file |
| --- | --- | --- | --- |
| `bg-yellow` | Page background around the main text | `#eee07b` | `images/bg-yellow.png` |
| `bg-maroon` | Background behind the banner and button | `#450311` | `images/bg-maroon.png` |
| `button-jelentkezz` | The **JELENTKEZZ ITT** button | Black text on `#eee07b` | `images/button-jelentkezz.png` |
| Banner, logos, social icons | | | `images/`, `psd/` |

**Why the backgrounds and the button are images:** in dark mode, Gmail, Outlook and Spark recolour every background and text colour in an email, but they leave images alone. Using images keeps the yellow, the dark red and the button looking the same in dark mode. The light-grey text panel is still a plain colour on purpose. Dark mode recolours the panel and its text together, so the text stays readable.

**If you change a colour**, make new 16×16 single-colour PNGs. Upload them, then update both the image URL and the matching `background-color` next to it in `index.html`. That colour is the fallback when images are blocked.

**If you change the button text or colours**, regenerate the button image. This script makes it at 3× size so it stays sharp on phones (it needs Python with Pillow, on macOS):

```python
from PIL import Image, ImageDraw, ImageFont
S, W, H = 3, 190, 40
img = Image.new("RGB", (W * S, H * S), "#eee07b")
font = ImageFont.truetype("/System/Library/Fonts/Supplemental/Arial Bold.ttf", 18 * S)
ImageDraw.Draw(img).text((W * S / 2, H * S / 2), "JELENTKEZZ ITT",
                         font=font, fill="#000000", anchor="mm")
img.save("images/button-jelentkezz.png", optimize=True)
```

Then upload the new button image and update the button's `src`, plus its `alt` and `title` text.

## 3. Send the email from Gmail

Gmail can't take raw HTML on its own. We use the **HTML Editor for Gmail by cloudHQ** browser extension.

1. Install the cloudHQ HTML editor extension for Gmail in Chrome.
2. In Gmail, open a new email and add the subject and recipients. For a mailing list, put the addresses in **BCC**.
3. **Turn off plain text mode.** In the compose window, click the three-dot **More options** menu at the bottom and make sure **Plain text mode** is **unchecked**. If it's checked, Gmail removes all the HTML and sends bare text.
4. Open the cloudHQ HTML editor from the compose window. Copy everything between `<body …>` and `</body>` in `index.html`, which is the `<table id="u_body">…</table>` block, paste it in, and insert it into the email.
5. **Send a test to yourself first.** Open it in Gmail on the web, in Gmail on your phone, and in Outlook if you can. Check each one in both light and dark mode.
6. Send it to the real recipients.

### Limitations

- **The `<style>` block in `<head>` isn't sent.** That's why every style in `index.html` is written onto each element, and why the layout works without the `<style>` block. Keep new styles inline too.
- **Only standard fonts work.** Gmail and Outlook don't load custom fonts, so stick to standard ones such as Arial and Georgia. Montserrat shows as a plain sans-serif in most apps.
- **Dark mode can't be fully controlled.** Images stay as they are, but text colours and the grey panel can still change in Outlook's and Gmail's apps. The email stays readable, and readers can usually turn dark mode off for a single email.
