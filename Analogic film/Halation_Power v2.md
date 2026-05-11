<div align="center"><h1>Halation Power DCTL</h1><p><strong>Version 1.0 | Analogical Halation Emulation</strong></p>
<p>Created by Galavnikov @ lakravana.com</strong></p></div><hr>
<h3>1. Logline</h3>
<p>
A high-performance analog halation emulator that recreates the signature red-glow light bleed of film by diffusing high-frequency luminance data through customizable Sigmoid or Logarithmic decay curves.
</p><hr><h3>2. The Creative Gap</h3>
<p>
In physical film, "Halation" occurs when bright light passes through the emulsion and reflects off the film base, scattering back into the red-sensitive layer. Most digital halation tools treat this as a simple "glow" or a blurred threshold.
</p>
<p>
The creative gap exists in the <strong>falloff control</strong>. Standard digital glows often look plastic because they lack a natural transition between the light source and the diffused glow. Colorists needed a tool that allows them to choose <em>how</em> the light bleeds—whether it follows a smooth, organic S-curve or a more aggressive logarithmic expansion—while maintaining precise control over the hue and spatial spread.
</p><hr><h3>3. The Solution & Mathematical Logic</h3><p>This script solves the "digital glow" problem by separating the halation into a multi-step spatial and mathematical process:</p><ul>
<li>
<strong>Luminance-Based Masking:</strong> The script analyzes the image using Rec.709 luma coefficients to isolate the areas that will "bleed". Instead of a hard threshold, it uses two distinct mathematical models to define the mask:
<ul>
<li><strong>Sigmoid (S-Curve):</strong> Uses a logistic function to create a smooth, natural transition.
$$Mask = \frac{1}{1 + e^{-k \cdot (Luma - m) \cdot 10}}$$</li>
<li><strong>Logarithmic:</strong> Uses a log function to aggressively push the glow into the mid-tones.
$$Mask = \frac{\ln(norm\_x \cdot C + 1)}{\ln(C + 1)}$$</li>
</ul>
</li>
<li>
<strong>Gaussian Diffusion:</strong> Rather than a simple blur, the mask is expanded using a Gaussian distribution. This ensures the "glow" has a mathematically correct physical decay, where the intensity drops off exponentially from the light source.
</li>
<li>
<strong>HSV Tinting & Additive Blending:</strong> The resulting blur is converted back into an RGB signal via a precise HSV-to-RGB conversion. This tinted signal is then <strong>additively</strong> combined with the original image, mimicking how light physically accumulates on a film frame.
</li>
</ul><hr><h3>4. GUI / Interface Elements</h3><table>
<thead>
<tr>
<th>Section</th>
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Enable</strong></td>
<td>Enable Halation</td>
<td>Master toggle to bypass or activate the halation effect.</td>
</tr>
<tr>
<td rowspan="4"><strong>Highlight Selection</strong></td>
<td>Highlight Midpoint</td>
<td>Sets the luminance threshold where halation begins to appear. Lower values allow halation to bleed from darker areas.</td>
</tr>
<tr>
<td>LOG intensity / SIGMO Strength</td>
<td>Adjusts the "hardness" or "softness" of the mask's edge based on the selected curve type.</td>
</tr>
<tr>
<td>Contrast Curve</td>
<td><strong>SIGMO:</strong> Organic, film-like falloff. <strong>LOG:</strong> Wide, dramatic bleed common in vintage lenses.</td>
</tr>
<tr>
</tr>
<tr>
<td><strong>Blur Radius</strong></td>
<td>Blur Radius</td>
<td>Determines the spatial spread of the glow. Higher values simulate more significant light scattering within the film layers.</td>
</tr>
<tr>
<td rowspan="2"><strong>Halation Color</strong></td>
<td>Halation Hue</td>
<td>Sets the color of the glow (0° is traditional film Red). Can be shifted for creative effects like sci-fi cyan glows.</td>
</tr>
<tr>
<td>Halation Intensity</td>
