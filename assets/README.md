# Assets / Recursos

Este directorio contiene los recursos visuales del perfil de GitHub.

## Banner Animado (banner.svg)

### Problema Original
El archivo SVG original referenciaba una imagen PNG externa usando `<image xlink:href="bg_lofi_anime.png" ... />`. Esto funcionaba correctamente en navegadores locales, pero **GitHub sanitiza los archivos SVG en los README** por razones de seguridad, eliminando referencias a recursos externos.

### Solución Implementada
Para que el banner funcione correctamente en GitHub README, la imagen PNG de fondo se ha **embebido directamente en el SVG como un data URI en formato base64**. Esto significa que la imagen está codificada dentro del mismo archivo SVG.

**Ventajas:**
- ✅ Funciona perfectamente en GitHub README
- ✅ Mantiene las animaciones SVG
- ✅ Un solo archivo, sin dependencias externas
- ✅ Funciona en cualquier plataforma que soporte SVG

**Desventajas:**
- ⚠️ Archivo más grande (~2.7MB vs 2KB del SVG original)
- ⚠️ No se puede modificar la imagen de fondo fácilmente (hay que regenerar el SVG)

### Alternativas Consideradas

1. **Usar solo SVG puro con gradientes/patrones** (sin imagen rasterizada)
   - Más ligero pero pierde el estilo específico de la imagen lofi anime

2. **Convertir a GIF animado**
   - Compatible con GitHub pero pierde calidad vectorial y tiene mayor tamaño

3. **Usar servicios externos** (ej: URLs absolutas de Imgur, Cloudinary)
   - Funciona pero depende de servicios externos y puede romper con el tiempo

### Cómo Actualizar la Imagen de Fondo

Si necesitas cambiar la imagen de fondo, sigue estos pasos:

```bash
# 1. Reemplaza la imagen PNG
cp nueva_imagen.png assets/assetsbg_lofi_anime.png

# 2. Regenera el SVG con imagen embebida
python3 << 'EOF'
import base64

# Leer la imagen PNG
with open('assets/assetsbg_lofi_anime.png', 'rb') as f:
    png_data = f.read()
    base64_data = base64.b64encode(png_data).decode('utf-8')

# Leer la plantilla SVG (o crear una nueva)
svg_template = '''<svg width="1152" height="768" viewBox="0 0 1152 768" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
  <image xlink:href="data:image/png;base64,{base64_data}" x="0" y="0" width="1152" height="768" />
  <!-- Tus animaciones y elementos SVG aquí -->
</svg>'''

# Generar el nuevo SVG
svg_content = svg_template.format(base64_data=base64_data)

# Guardar
with open('assets/banner.svg', 'w') as f:
    f.write(svg_content)
EOF

# 3. Commit y push
git add assets/banner.svg
git commit -m "Update banner background image"
git push
```

### Recursos Adicionales

- [GitHub's SVG Sanitization](https://github.com/github/markup/issues/556)
- [Data URIs en SVG](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)
- [SVG Animations](https://developer.mozilla.org/en-US/docs/Web/SVG/SVG_animation_with_SMIL)

### Archivos en este Directorio

- **banner.svg** - Banner animado con imagen de fondo embebida (usar en README)
- **assetsbg_lofi_anime.png** - Imagen original de fondo (referencia para futuras actualizaciones)
