---
title: "Cable Lengths for Metric Brains: Feet, Metres and a Converter"
description: Patch cables are sold in feet, racks are measured in inches and units, and distance limits are in metres. Easy rules to remember, a reference table, the distance limits that matter, and a converter you can use on the spot.
tags:
  - Cabling
  - Data Center
  - Reference
  - Tools
---

# Cable Lengths for Metric Brains: Feet, Metres and a Converter

I grew up with metres and centimetres. Then I started working in North American data rooms,
where patch cables come in **3, 5, 7 and 14 feet**, racks are **19 inches** wide, and someone on
the phone says the run is "about 60 feet". Meanwhile the standards and optics datasheets still
say **100 m** and **300 m**.

So I keep a few rules in my head, and a converter for everything else. Both are below.

!!! abstract "Key takeaway"
    **1 m ≈ 3.3 ft** (3 ft 3 in), **1 ft ≈ 30 cm**, **10 ft ≈ 3 m**, **1 in = 2.54 cm**, and the
    copper limit **100 m = 328 ft**. For a patch cable, measure the real path (along the tray
    and down to the port), add **about 10%** for slack, then pick the **next standard size up**.
    The converter at the bottom does the arithmetic, and suggests a cable size.

![Easy metric-to-feet rules, common cable sizes and distance limits](../assets/images/cable-lengths-metric-feet.svg)

## Rules worth memorizing

| Rule | Exact | Good enough in the field |
|---|---|---|
| Metre → feet | 1 m = 3.281 ft | **× 3.3**, or "3 ft and a bit" |
| Feet → metres | 1 ft = 0.3048 m | **× 0.3**, or "a foot is a 30 cm ruler" |
| Ten feet | 10 ft = 3.05 m | **10 ft ≈ 3 m** |
| One yard | 1 yd = 3 ft = 0.914 m | **a yard is a bit less than a metre** |
| Inch | 1 in = 2.54 cm | **4 in ≈ 10 cm** |
| Copper channel | 100 m = 328 ft | **"328" is the number you'll hear** |
| Rack unit | 1U = 1.75 in = 44.45 mm | **about 4.5 cm per U** |
| Rack width | 19 in = 482.6 mm | the "19-inch rack" |

The trick that helped me most: think of **feet as 30 cm rulers**. A 7 ft cable is seven rulers,
about 2.1 m. A 14 ft cable is about 4.3 m.

## Common patch cable sizes

Sizes in North America are usually in feet. Elsewhere they're in metres, and the two don't
line up exactly, which matters when you're replacing a cable from a different supplier.

| Imperial size | In metres | Nearest metric size |
|---|---|---|
| 1 ft | 0.30 m | 0.3 m |
| 3 ft | 0.91 m | 1 m |
| 5 ft | 1.52 m | 1.5 m |
| 7 ft | 2.13 m | 2 m |
| 10 ft | 3.05 m | 3 m |
| 14 ft | 4.27 m | 5 m (or 4 m, if it's tight) |
| 25 ft | 7.62 m | 7.5–8 m |
| 50 ft | 15.24 m | 15 m |

## Distance limits that matter

| Link | Media | Maximum distance |
|---|---|---|
| 1000BASE-T, 10GBASE-T | Cat 5e / Cat 6a copper | **100 m (328 ft)** channel: up to 90 m (295 ft) of fixed cable plus 10 m (33 ft) of patch cords |
| 10GBASE-T | Cat 6 unshielded | 55 m (180 ft) |
| 1000BASE-SX | OM2 multimode | 550 m |
| 10GBASE-SR | OM3 / OM4 multimode | 300 m / 400 m |
| 40GBASE-SR4 | OM3 / OM4 multimode | 100 m / 150 m |
| 100GBASE-SR4 | OM3 / OM4 multimode | 70 m / 100 m |
| 10GBASE-LR | Single-mode | 10 km |

These are the standard limits. Real links also lose length to patch panels, splices and dirty
connectors, so leave a margin, and check the optics vendor's datasheet for your exact module
(see [SFP Types and Compatibility](sfp-types-compatibility.md)).

## Measuring a run properly

1. **Follow the path, not the straight line.** Up into the tray, along it, and down the rack
   to the port. Straight-line distance is often half the real length.
2. **Add the vertical drops.** Each rack is about 2 m (7 ft) tall for a 42U rack.
3. **Add about 10%** for slack and bends.
4. **Pick the next standard size up.** A cable that's too short can't be fixed; one that's a
   little long can be dressed neatly.

## Converter

<style>
.l3-conv{border:1px solid var(--md-default-fg-color--lightest);border-radius:.6rem;padding:1rem 1.2rem;margin:1rem 0;background:var(--md-code-bg-color)}
.l3-conv .row{display:flex;flex-wrap:wrap;gap:.8rem;align-items:flex-end;margin-bottom:.9rem}
.l3-conv label{display:flex;flex-direction:column;font-size:.72rem;font-weight:700;color:var(--md-default-fg-color--light);min-width:7.5rem;flex:1}
.l3-conv input{font:inherit;font-size:.85rem;padding:.4rem .5rem;margin-top:.25rem;border:1px solid var(--md-default-fg-color--lighter);border-radius:.3rem;background:var(--md-default-bg-color);color:var(--md-default-fg-color);width:100%;box-sizing:border-box}
.l3-conv input:focus{outline:2px solid var(--md-accent-fg-color);outline-offset:1px}
.l3-conv .eq{font-size:1.1rem;color:var(--md-default-fg-color--light);padding-bottom:.45rem;flex:0}
.l3-conv h4{margin:.2rem 0 .6rem;font-size:.8rem}
.l3-conv .out{font-size:.8rem;line-height:1.6;margin:0}
.l3-conv .out strong{color:var(--md-accent-fg-color)}
.l3-conv hr{margin:.8rem 0;border:0;border-top:1px solid var(--md-default-fg-color--lightest)}
</style>

<div class="l3-conv" id="l3-conv">
<h4>Length: metres ⇄ feet</h4>
<div class="row">
<label>Metres<input id="c-m" type="number" inputmode="decimal" min="0" step="any" value="3"></label>
<span class="eq">=</span>
<label>Feet (decimal)<input id="c-ft" type="number" inputmode="decimal" min="0" step="any"></label>
<span class="eq">=</span>
<label>Feet + inches<input id="c-ftin" type="text" readonly></label>
</div>
<h4>Small sizes: centimetres ⇄ inches</h4>
<div class="row">
<label>Centimetres<input id="c-cm" type="number" inputmode="decimal" min="0" step="any" value="10"></label>
<span class="eq">=</span>
<label>Inches<input id="c-in" type="number" inputmode="decimal" min="0" step="any"></label>
<span class="eq">=</span>
<label>Rack units (U)<input id="c-u" type="text" readonly></label>
</div>
<hr>
<h4>Which patch cable should I buy?</h4>
<div class="row">
<label>Measured path length (metres)<input id="c-run" type="number" inputmode="decimal" min="0" step="any" value="3.5"></label>
<label>Slack to add (%)<input id="c-slack" type="number" inputmode="decimal" min="0" step="1" value="10"></label>
</div>
<p class="out" id="c-pick"></p>
</div>

<script>
(function () {
  var root = document.getElementById("l3-conv");
  if (!root) return;
  function $(id) { return document.getElementById(id); }
  var M_PER_FT = 0.3048, CM_PER_IN = 2.54, MM_PER_U = 44.45;
  var FT_SIZES = [1, 2, 3, 5, 7, 10, 14, 15, 20, 25, 30, 50, 75, 100];
  var M_SIZES = [0.3, 0.5, 1, 1.5, 2, 3, 4, 5, 7.5, 10, 15, 20, 30];
  function num(el) { var v = parseFloat(el.value); return isFinite(v) && v >= 0 ? v : null; }
  function round(v, d) { var p = Math.pow(10, d); return Math.round(v * p) / p; }
  function ftIn(ft) {
    var totalIn = Math.round(ft * 12);
    return Math.floor(totalIn / 12) + " ft " + (totalIn % 12) + " in";
  }
  function fromM() {
    var m = num($("c-m"));
    if (m === null) { $("c-ft").value = ""; $("c-ftin").value = ""; return; }
    var ft = m / M_PER_FT;
    $("c-ft").value = round(ft, 2);
    $("c-ftin").value = ftIn(ft);
  }
  function fromFt() {
    var ft = num($("c-ft"));
    if (ft === null) { $("c-m").value = ""; $("c-ftin").value = ""; return; }
    $("c-m").value = round(ft * M_PER_FT, 3);
    $("c-ftin").value = ftIn(ft);
  }
  function showU(inches) {
    $("c-u").value = inches === null ? "" : round(inches * CM_PER_IN * 10 / MM_PER_U, 2) + " U";
  }
  function fromCm() {
    var cm = num($("c-cm"));
    if (cm === null) { $("c-in").value = ""; showU(null); return; }
    var inches = cm / CM_PER_IN;
    $("c-in").value = round(inches, 2);
    showU(inches);
  }
  function fromIn() {
    var inches = num($("c-in"));
    if (inches === null) { $("c-cm").value = ""; showU(null); return; }
    $("c-cm").value = round(inches * CM_PER_IN, 2);
    showU(inches);
  }
  function pick() {
    var run = num($("c-run")), slack = num($("c-slack"));
    var out = $("c-pick");
    if (run === null || slack === null) { out.textContent = "Enter a length and a slack percentage."; return; }
    var need = run * (1 + slack / 100);
    var needFt = need / M_PER_FT;
    var ft = FT_SIZES.filter(function (s) { return s >= needFt - 1e-9; })[0];
    var m = M_SIZES.filter(function (s) { return s >= need - 1e-9; })[0];
    var html = "You need about <strong>" + round(need, 2) + " m</strong> (" + round(needFt, 1) + " ft). ";
    html += ft ? "Imperial: buy <strong>" + ft + " ft</strong> (" + round(ft * M_PER_FT, 2) + " m). "
               : "Longer than standard imperial patch cables. ";
    html += m ? "Metric: buy <strong>" + m + " m</strong>." : "Longer than standard metric patch cables.";
    if (need > 100) {
      html += "<br>Over <strong>100 m</strong>: too long for a copper Ethernet channel. Use fibre.";
    } else if (need > 10) {
      html += "<br>Over 10 m: copper patch cords in a channel should total 10 m or less, so this is usually a fixed cable run.";
    }
    out.innerHTML = html;
  }
  $("c-m").addEventListener("input", fromM);
  $("c-ft").addEventListener("input", fromFt);
  $("c-cm").addEventListener("input", fromCm);
  $("c-in").addEventListener("input", fromIn);
  $("c-run").addEventListener("input", pick);
  $("c-slack").addEventListener("input", pick);
  fromM(); fromCm(); pick();
})();
</script>

## Lessons learned

!!! tip "Takeaways"
    - **1 m ≈ 3.3 ft**, **1 ft ≈ 30 cm**, **10 ft ≈ 3 m**, **1 in = 2.54 cm**.
    - Copper Ethernet: **100 m (328 ft)** for the whole channel, including patch cords.
    - Imperial and metric cable sizes don't match exactly. A 7 ft cable is 2.13 m, not 2 m.
    - Measure the real path, add about 10%, and round **up** to the next standard size.

## References

- University of Wisconsin KnowledgeBase: [Ethernet media standards and distances](https://kb.wisc.edu/7829)
- Ethernet Alliance: [10GbE standardized to 400 metres on OM4 fiber](https://ethernetalliance.org/blog/2012/06/13/10gbe-standardized-to-400-meters-on-om4-fiber/)
- TrueCABLE: [Calculating Ethernet cable overall channel length](https://www.truecable.com/blogs/cable-academy/calculating-ethernet-cable-overall-channel-length-for-success)
