---
title: Phreak (CLI Builder for Crystal)
date:  2020-12-01 12:00:00
categories:
  - Project
  - Crystal
  - Linux
tags:
  - crystal
  - linux
---

## Overview
During the late fall of 2019, I was in the process of developing a small command-line interface (CLI)
to make navigating between project directories faster. To do this, I was using [Crystal's](https://www.crystal-lang.org/)
`OptionParser` API - a CLI building tool bundled with the standard library. However, I found myself
unsatisfied with the amount of control it provided - it could not natively support subcommands (think `git add ...`) or
provide autocorrection (understanding `git ad` as `git add`). As a result, I ended up taking a substantial detour to
develop [Phreak](https://github.com/shinzlet/Phreak/), a CLI builder library which provided far more features than
OptionParser did at the time.

## Example
Here's an example [from the documentation](https://github.com/shinzlet/phreak#nested-subcommands) showing what Phreak can do.

```crystal
require "phreak"

Phreak.parse! do |root|
  root.bind(word: "wifi", description: "Configure or control wireless connections.") do |wifi|
    wifi.banner = "Manage wireless connections. Invocation: cli wifi [subcommand]"

    wifi.bind(word: "status", description: "Get the current status of the modem.") do
      # Reponds to "nested wifi status"
    end

    wifi.bind(word: "set", description: "Set the the wifi state.") do |set|
      # Responds to "nested wifi set"
    end

    wifi.bind(word: "help",
              description: "display specific help about the `wifi` subcommand.") do |help|
      puts wifi
    end
  end

  root.bind(word: "help", description: "display this help menu.") do |help|
    puts root
  end
end
```

To pull off semantics like that using (2019's version of) `OptionParser`, you would have had to manually detect
the subcommands by parsing `ARGV`, then delegate only the final step (parsing flags) to one of many `OptionParser`
instances. I was really happy with how much Phreak improved my productivity when working on test projects.

## Conclusion & Phreak's Future
Phreak is currently my most popular open source project - It has 25 stars on github at the time of writing,
and I have heard from several people who have, at the very least, played around with it.

Although I'll likely be the only one to tell you this, I also believe that Phreak has left it's fingerprints on the
Crystal ecosystem. [RX14](https://github.com/RX14), a Crystal maintainer, [left a comment](https://github.com/veelenga/awesome-crystal/pull/449#issuecomment-547630728)
sharing her support of how Phreak handled subcommands. She convinced me to open an [issue](https://github.com/crystal-lang/crystal/issues/8656)
on the Crystal repository, which garnered mostly positive reactions. That issue is open to this day, but
RX14 and others have since rewritten OptionParser to use Phreak-like idioms, and to implement Phreak-like
features. Of course, I do not claim to be the sole cause of this, and I certainly can't attribute any of that
skillful work to my hand. However, as a teenager dipping their toes into the waters of open source for the first time,
I was really excited to have left an impression on a project that I love and believe in so dearly.

With a stronger Crystal standard library now in place, Phreak is now mostly irrelevant. Still, it will always have
a special place in my heart. It helped my build my confidence
with OSS, taught me how to document code for people I would never meet, and encouraged me to work on bigger
and better things.