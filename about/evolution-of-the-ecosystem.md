---
description: "How Sākṣī evolved from a single idea into a multi-component architecture."
icon: sitemap
layout:
  description:
    visible: false
---
# Evolution of the Ecosystem

So, as I mentioned in [The Origin Story](the-origin-story.md), my first idea was to just save the files inside the camera app. But that was risky because if someone just deleted the camera app, all the evidence would be gone too.

I was stuck trying to figure out how to hide the files safely.

## The Background Application Concept

The answer came from something I did when I was a kid. Back then, I was super curious and always messing around with phones—just tinkering to see how things worked.

During one of those times, I found this weird app installed on the phone. It had no name, no logo, and it didn't even show up in the app drawer. When I went into the phone's settings to look at the installed apps list, there was just a blank space where an app should be.

Turns out, it was a malware app💀. And yes, I was the one who installed it😶‍🌫️. I had no idea what it was back then. I was just doing random stuff. But remembering that got me thinking!

What if I build a background service app like that? An app that just runs quietly, without a logo or an icon in the app drawer. It wouldn't be malware, obviously, but a secure vault.

This became the core idea:
1. **The Camera:** You use one app to record.
2. **The Vault:** The video is secretly sent to this background app. Fragmented MP4 (fMP4) is continuously saved while recording, but Normal MP4 is copied only after recording stops.

If someone checks your app drawer, they won't see a "Vault" app. They'll just see the Camera. If they force you to delete the video from your gallery, that's fine. The Vault still has a copy.

## The Need for an Interface

But this created a new problem. If the Vault is basically invisible and running in the background, how am I supposed to look at my files later?

I needed a way to interact with the Vault without ruining its hidden nature. That's how **Sākṣī Portal** was born.

The Portal is just an interface. You install it, use it to connect to the Vault, view your videos, import new files into Vault, or export them back to your gallery. And the best part? Once you are done, you can just uninstall the Portal. The Vault stays hidden in the background, keeping your files safe until you need them again.

## The Resulting Ecosystem

What started as just a thought about stopping people from deleting videos turned into a full system.

Now we have:

| Component | What it does |
| :--- | :--- |
| **[Sākṣī Camera](../overview/README.md#saksi-camera)** | Captures the moments and sends them to the Vault based on the recording mode. |
| **[Sākṣī Vault](../overview/README.md#saksi-vault)** | The silent background worker inspired by that weird childhood app. It just holds your files. |
| **[Sākṣī Portal](../overview/README.md#saksi-portal)** | Your window into the Vault. Install it when you need it, delete it when you don't. |
| **[Sākṣī SDK](../developer-guide/sdk/integration.md)** | The behind-the-scenes magic that lets the Camera and Portal talk to the Vault securely. |

It took a lot of thought, but splitting it up like this makes the whole thing much safer and more flexible.

---

*(Note: I ([Rajnish Kumar](about-the-developer.md)) wrote this original story in Hindi, and the AI translated it into English for you to read here.)*
