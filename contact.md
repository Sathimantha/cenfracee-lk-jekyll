---
layout: default
title: Contact — Cenfracee
---

<section class="section">
  <div class="container">
    <div class="contact-grid">

      <!-- Left: info -->
      <div>
        <p class="label teal anim-1" style="margin-bottom:14px;">// Get in Touch</p>
        <h1 class="d-lg anim-2" style="margin-bottom:32px;">LET'S<br>BUILD<br>TOGETHER.</h1>
        <p class="anim-3" style="font-size:.9rem;color:var(--muted);line-height:1.9;max-width:36ch;margin-bottom:48px;">
          Partnership proposals, technical collaboration, investment inquiries,
          or just a conversation about what we're building.
        </p>

        <!-- contact details -->
        <div class="anim-4" style="border-top:1px solid var(--border);padding-top:32px;">
          <div style="margin-bottom:28px;">
            <p class="label muted" style="margin-bottom:6px;font-size:.58rem;">Email</p>
            <a href="mailto:hello@cenfracee.com" class="mono" style="font-size:.85rem;color:var(--teal);transition:color .2s;"
              onmouseenter="this.style.color='var(--amber)'" onmouseleave="this.style.color='var(--teal)'">
              hello@cenfracee.com
            </a>
          </div>
          <div style="margin-bottom:28px;">
            <p class="label muted" style="margin-bottom:6px;font-size:.58rem;">Location</p>
            <p class="mono" style="font-size:.8rem;color:var(--text);">Sri Lanka</p>
          </div>
          <div>
            <p class="label muted" style="margin-bottom:6px;font-size:.58rem;">Web</p>
            <p class="mono" style="font-size:.8rem;color:var(--text);">cenfracee.com</p>
          </div>
        </div>

        <!-- element decoration -->
        <div class="anim-5" style="margin-top:56px;">
          <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:1px;max-width:200px;">
            {% assign elements = "Fe,C,Au,Si" | split: "," %}
            {% for el in elements %}
            <div style="background:var(--bg2);border:1px solid var(--border);padding:8px;text-align:center;">
              <div class="mono amber" style="font-size:.9rem;">{{ el }}</div>
            </div>
            {% endfor %}
          </div>
        </div>
      </div>

      <!-- Right: form -->
      <div class="anim-3" style="background:var(--bg2);border:1px solid var(--border);padding:48px;">
        <p class="label" style="margin-bottom:32px;color:var(--muted);">Send a message</p>

        <form name="contact" netlify style="display:flex;flex-direction:column;">
          <div class="form-field">
            <label class="form-label" for="name">Your Name</label>
            <input class="form-input" type="text" id="name" name="name" placeholder="Enter your name" required />
          </div>

          <div class="form-field">
            <label class="form-label" for="email">Email Address</label>
            <input class="form-input" type="email" id="email" name="email" placeholder="you@example.com" required />
          </div>

          <div class="form-field">
            <label class="form-label" for="subject">Subject</label>
            <input class="form-input" type="text" id="subject" name="subject" placeholder="Partnership / Technical / Other" />
          </div>

          <div class="form-field">
            <label class="form-label" for="message">Message</label>
            <textarea class="form-input" id="message" name="message" rows="6"
              placeholder="Tell us what you're working on..."
              style="resize:vertical;" required></textarea>
          </div>

          <button type="submit" class="btn btn-amber" style="margin-top:12px;justify-content:center;font-size:.7rem;">
            Send Message →
          </button>
        </form>

        <p class="mono" style="font-size:.62rem;color:var(--dim);letter-spacing:.1em;margin-top:20px;text-align:center;">
          OR EMAIL DIRECTLY AT
          <a href="mailto:hello@cenfracee.com" style="color:var(--amber);">HELLO@CENFRACEE.COM</a>
        </p>
      </div>

    </div>
  </div>
</section>
