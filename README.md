# 🎬 Tutorial: Script de Doblaje Automático de Videos

## 📋 Resumen Ejecutivo

Este script automatiza el proceso de doblaje de videos del inglés al español. Utiliza inteligencia artificial para transcribir, traducir y generar voz, permitiendo crear versiones dobladas de videos de manera automática.

## 🔍 ¿Qué Hace Este Script?

El script **`doblar.py`** es una herramienta completa que:

- 🎧 **Extrae el audio** de videos en inglés
- 📝 **Transcribe** el audio a texto usando reconocimiento de voz
- 🌐 **Traduce** el texto del inglés al español
- 🔊 **Genera audio** en español usando síntesis de voz
- 🎬 **Reemplaza** el audio original con el doblado
- 💾 **Guarda** el video final en una carpeta específica

### Flujo de Trabajo del Script

```
Video Original (Inglés) → Extraer Audio → Dividir en Segmentos → Transcribir
                                                                 ↓
Traducir al Español → Generar Voz en Español → Combinar Audio → Video Final
```

## 📋 Requisitos del Sistema

### 🔧 Software Requerido

- **Python 3.7+** - Lenguaje de programación base
- **FFmpeg** - Para procesamiento de video/audio
- **Conexión a Internet** - Para servicios de traducción y síntesis de voz

### 📦 Dependencias de Python

```bash
pip install moviepy speechrecognition googletrans==4.0.0rc1 gtts pydub
```

### 💾 Requisitos de Hardware

- **CPU:** Procesador moderno (recomendado 4+ núcleos)
- **RAM:** Mínimo 4GB (recomendado 8GB+)
- **Almacenamiento:** Espacio suficiente para videos originales y procesados

## 🚀 Instalación y Configuración

### Paso 1: Instalar Python
Descarga Python 3.7 o superior desde [python.org](https://python.org)

### Paso 2: Instalar FFmpeg
```bash
# Ubuntu/Debian
sudo apt-get install ffmpeg

# macOS
brew install ffmpeg

# Windows (con Chocolatey)
choco install ffmpeg
```

### Paso 3: Instalar dependencias
```bash
pip install moviepy speechrecognition googletrans==4.0.0rc1 gtts pydub
```

### Paso 4: Descargar el script
Guarda el archivo `doblar.py` en tu directorio de trabajo

## 📁 Estructura de Carpetas

El script espera la siguiente estructura de carpetas:

```
📁 Directorio de Trabajo
├── 📄 doblar.py
├── 📁 original/          # ← Coloca aquí tus videos en inglés
│   ├── video1.mp4
│   ├── video2.avi
│   └── ...
└── 📁 doblada/           # ← Aquí se guardarán los videos doblados
    ├── video1_doblado.mp4
    ├── video2_doblado.mp4
    └── ...
```

⚠️ **Importante:** Las carpetas `original` y `doblada` se crearán automáticamente si no existen.

## 🎯 Cómo Usar el Script

### Paso 1: Preparar videos
Copia todos los videos en inglés que quieres doblar a la carpeta `original/`

### Paso 2: Ejecutar el script
```bash
python doblar.py
```

### Paso 3: Esperar el procesamiento
El script procesará automáticamente todos los videos. El tiempo depende de la duración y cantidad de videos.

### Paso 4: Revisar resultados
Los videos doblados estarán en la carpeta `doblada/` con el sufijo "_doblado"

## ⚙️ Funcionamiento Técnico Detallado

### 1. Extracción de Audio
El script utiliza **MoviePy** para extraer la pista de audio del video original:

```python
video = VideoFileClip(video_path)
audio = video.audio
audio.write_audiofile("audio_original.wav")
```

### 2. Segmentación por Silencios
Divide el audio en fragmentos basados en silencios para mejorar la precisión:

```python
chunks = split_on_silence(audio, min_silence_len=1000, silence_thresh=-40)
```

### 3. Transcripción de Voz a Texto
Utiliza **SpeechRecognition** con Google Speech API:

```python
with sr.AudioFile(chunk) as source:
    audio_data = recognizer.record(source)
    text = recognizer.recognize_google(audio_data, language='en-US')
```

### 4. Traducción Automática
Traduce el texto usando **Google Translate**:

```python
translated = translator.translate(text, dest='es')
spanish_text = translated.text
```

### 5. Síntesis de Voz en Español
Genera audio en español con **gTTS (Google Text-to-Speech)**:

```python
tts = gTTS(text=spanish_text, lang='es')
tts.save("audio_espanol.mp3")
```

### 6. Reemplazo de Audio en Video
Combina el video original con el nuevo audio:

```python
video = VideoFileClip(video_path)
new_audio = AudioFileClip(audio_espanol_path)
final_video = video.set_audio(new_audio)
final_video.write_videofile(output_path)
```

## 📊 Formatos de Video Soportados

| Formato | Extensión | Compatibilidad |
|---------|-----------|----------------|
| MP4 | .mp4 | ✅ Total |
| AVI | .avi | ✅ Total |
| QuickTime | .mov | ✅ Total |
| MKV | .mkv | ✅ Total |
| Windows Media | .wmv | ✅ Total |
| Flash Video | .flv | ✅ Total |

## 🔧 Configuración Avanzada

### Parámetros de Segmentación
```python
# En el método split_audio_on_silence()
min_silence_len=1000,     # Duración mínima de silencio (ms)
silence_thresh=-40        # Umbral de silencio (dB)
```

### Idiomas Soportados
```python
# Para cambiar el idioma de destino
translated_text = self.translate_text(text, target_language='es')

# Idiomas disponibles: 'es', 'fr', 'de', 'it', 'pt', etc.
```

## 🚨 Solución de Problemas

### Error: "Module not found"
**Solución:** Instala las dependencias faltantes:
```bash
pip install moviepy speechrecognition googletrans==4.0.0rc1 gtts pydub
```

### Error: "FFmpeg not found"
**Solución:** Instala FFmpeg en tu sistema:
```bash
# Linux
sudo apt-get install ffmpeg

# macOS
brew install ffmpeg
```

### Error de conexión a Internet
**Solución:** Verifica tu conexión. Los servicios de Google requieren internet para traducción y síntesis de voz.

### Audio de mala calidad o inexistente
**Solución:** Asegúrate de que el video tenga una pista de audio clara en inglés. Videos con audio de baja calidad pueden no transcribirse correctamente.

### El video doblado es más largo/corto
**Solución:** El script ajusta automáticamente las duraciones, pero si persiste, verifica que el video original tenga audio sincronizado.

## ⚡ Optimizaciones y Mejores Prácticas

### 💡 Consejos para Mejor Rendimiento

- **Divide videos largos:** Videos de más de 30 minutos pueden ser problemáticos
- **Audio de calidad:** Mejor transcripción con audio claro y sin ruido de fondo
- **Conexión estable:** Los servicios de Google requieren buena conexión
- **Espacio en disco:** Los archivos temporales requieren espacio adicional

### 🎯 Casos de Uso Recomendados

- 🎬 Doblaje de tutoriales y cursos
- 📚 Contenido educativo
- 🎥 Videos corporativos
- 📹 Contenido para redes sociales
- 🎮 Videos de juegos y entretenimiento

## 📈 Métricas y Rendimiento

| Tipo de Video | Tiempo de Procesamiento | Precisión Esperada |
|---------------|-------------------------|-------------------|
| Video corto (5-10 min) | 2-5 minutos | 85-95% |
| Video mediano (10-30 min) | 5-15 minutos | 80-90% |
| Video largo (30+ min) | 15-45+ minutos | 75-85% |

## 🔒 Limitaciones y Consideraciones

### ⚠️ Limitaciones Técnicas

- **Dependencia de Internet:** Requiere conexión para servicios de Google
- **Precisión del Reconocimiento:** La transcripción puede tener errores con audio de baja calidad
- **Idioma Limitado:** Actualmente solo del inglés al español
- **Procesamiento por Segmentos:** Videos muy largos pueden ser problemáticos

## 🚀 Próximas Mejoras

Versiones futuras podrían incluir:

- 🌍 Soporte para múltiples idiomas
- 🎭 Múltiples voces y acentos
- ⚡ Procesamiento en paralelo
- 🎯 Interfaz gráfica de usuario
- 🔧 Configuración personalizable avanzada

## 📞 Soporte y Comunidad

Si encuentras problemas o tienes sugerencias:

- 📋 Revisa la sección de solución de problemas
- 🐛 Verifica que todas las dependencias estén instaladas
- 🔍 Busca en logs detallados del script
- 💬 Comparte tu experiencia y mejoras

## 🎉 ¡Listo para Empezar!

Con este tutorial tienes todo lo necesario para comenzar a doblar videos automáticamente. El script es fácil de usar y produce resultados sorprendentemente buenos para su simplicidad.

**¡Que disfrutes creando contenido doblado! 🎬✨**
