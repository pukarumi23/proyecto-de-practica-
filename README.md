=== FILE: README.md ===
# 👋 Hola, soy TU_NOMBRE (@TU_USUARIO)

![header](https://img.shields.io/badge/Perfil-Dinámico-brightgreen)

> Soy desarrollador/a, creador/a de bots y amante de las aves.  
> Me gusta construir cosas, aprender y compartir.

---

## 🔭 Actualmente
**Estado:** _{{STATUS}}_  
**Última actualización:** **{{LAST_UPDATED}}**

---

## 🛠️ Tecnologías
- JavaScript / Node.js
- HTML / CSS
- Git / GitHub

---

## 📊 Estadísticas
<!-- Reemplaza USERNAME en las URLs por tu usuario -->
![GitHub Streak](https://github-readme-streak-stats.herokuapp.com/?user=TU_USUARIO&theme=dark)
![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=TU_USUARIO&layout=compact)

---

## 🎧 Ahora escuchando
> _{{NOW_PLAYING}}_

---

## 💬 Sobre mí
Me interesa automatización, bots para WhatsApp y mejorar mis proyectos con pruebas y CI/CD.

---

## 📫 Contáctame
- Email: tu.email@ejemplo.com
- GitHub: https://github.com/TU_USUARIO

---

> _Este README se actualiza automáticamente con GitHub Actions (ver `.github/workflows/update-readme.yml`)._

=== FILE: .github/workflows/update-readme.yml ===
name: Actualizar README dinámico

on:
  schedule:
    - cron: '0 12 * * *' # se ejecuta todos los días a las 12:00 UTC (ajusta si quieres)
  workflow_dispatch: {} # permite ejecutarlo manualmente desde Actions

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - name: Clonar repo
        uses: actions/checkout@v4
        with:
          persist-credentials: true

      - name: Dar permisos al script
        run: |
          chmod +x .github/scripts/update_readme.sh

      - name: Ejecutar script de actualización
        run: ./.github/scripts/update_readme.sh

      - name: Commit y push (si hubo cambios)
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          if [ -n "$(git status --porcelain)" ]; then
            git add README.md
            git commit -m "chore: actualizar README dinámico"
            git push
          else
            echo "No hay cambios para commitear"
          fi

=== FILE: .github/scripts/update_readme.sh ===
#!/usr/bin/env bash
# Script sencillo que actualiza dos placeholders en README.md:
#  - {{LAST_UPDATED}} -> fecha actual
#  - {{STATUS}} -> estado aleatorio de ejemplo
#  - {{NOW_PLAYING}} -> canción/actividad aleatoria de ejemplo

README="README.md"
DATE="$(date -u +"%Y-%m-%d %H:%M UTC")"

# Array de estados de ejemplo (personaliza)
STATUSES=("Trabajando en un bot 🤖" "Aprendiendo nuevas APIs 📚" "Tomando un café ☕" "Revisando PRs 🧩")
# Array de "now playing" de ejemplo
NOWS=("Lo-fi beats - playlist" "Hatsune Miku - Project" "Podcast: Tecnología Hoy" "Silencio productivo")

# Selección aleatoria
RND_STATUS=${STATUSES[$RANDOM % ${#STATUSES[@]}]}
RND_NOW=${NOWS[$RANDOM % ${#NOWS[@]}]}

# Reemplazar placeholders en README.md usando sed (crea copia de seguridad)
if [ -f "$README" ]; then
  # Reemplazo simple: si existen los placeholders los actualiza; si no, los añade al final.
  if grep -q "{{LAST_UPDATED}}" "$README"; then
    sed -i "s/{{LAST_UPDATED}}/$DATE/g" "$README"
  else
    # añade si no existe (evita duplicados)
    sed -i "1i Última actualización: **$DATE**\n" "$README"
  fi

  if grep -q "{{STATUS}}" "$README"; then
    sed -i "s/{{STATUS}}/$RND_STATUS/g" "$README"
  else
    sed -i "1i Estado: _${RND_STATUS}_\n" "$README"
  fi

  if grep -q "{{NOW_PLAYING}}" "$README"; then
    sed -i "s/{{NOW_PLAYING}}/$RND_NOW/g" "$README"
  else
    sed -i "1i Ahora escuchando: _${RND_NOW}_\n" "$README"
  fi

  echo "README actualizado: fecha -> $DATE, status -> $RND_STATUS, now -> $RND_NOW"
else
  echo "No se encontró $README"
  exit 1
fi

# FIN del script
