#!/bin/bash

# Colores para mensajes
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

echo -e "${GREEN}=== Script de configuración y sincronización de repositorio Git ===${NC}"

# Función para pausar y esperar
pause() {
    read -p "Presiona Enter para continuar..."
}

# 1. Verificar configuración global de Git
echo -e "\n${YELLOW}▶ Verificando configuración global de Git...${NC}"
GIT_USER=$(git config --global --get user.name)
GIT_EMAIL=$(git config --global --get user.email)

if [ -z "$GIT_USER" ] || [ -z "$GIT_EMAIL" ]; then
    echo -e "${RED}No se encontró configuración global completa.${NC}"
    read -p "Introduce tu nombre de usuario de GitHub: " new_user
    read -p "Introduce tu email de GitHub: " new_email
    git config --global user.name "$new_user"
    git config --global user.email "$new_email"
    echo -e "${GREEN}✓ Configuración global guardada.${NC}"
else
    echo -e "${GREEN}✓ Configuración global encontrada:${NC}"
    echo "  Usuario: $GIT_USER"
    echo "  Email: $GIT_EMAIL"
fi
pause

# 2. Verificar si es un repositorio Git
echo -e "\n${YELLOW}▶ Verificando repositorio Git...${NC}"
if [ ! -d ".git" ]; then
    echo -e "${YELLOW}No es un repositorio Git. Inicializando...${NC}"
    git init
    if [ $? -eq 0 ]; then
        echo -e "${GREEN}✓ Repositorio inicializado.${NC}"
    else
        echo -e "${RED}✗ Error al inicializar el repositorio.${NC}"
        exit 1
    fi
else
    echo -e "${GREEN}✓ Ya es un repositorio Git.${NC}"
fi
pause

# 3. Verificar rama actual
echo -e "\n${YELLOW}▶ Verificando rama actual...${NC}"
CURRENT_BRANCH=$(git branch --show-current 2>/dev/null)

if [ -z "$CURRENT_BRANCH" ]; then
    echo -e "${YELLOW}No hay commits todavía. Vamos a crear la rama inicial.${NC}"
    read -p "Nombre de la rama inicial (main/master): " INIT_BRANCH
    if [ -z "$INIT_BRANCH" ]; then
        INIT_BRANCH="main"
    fi
    git checkout -b "$INIT_BRANCH"
    CURRENT_BRANCH="$INIT_BRANCH"
    echo -e "${GREEN}✓ Rama creada: $CURRENT_BRANCH${NC}"
else
    echo -e "${GREEN}✓ Rama actual: $CURRENT_BRANCH${NC}"
fi

# 4. Manejo de rama master vs main
if [ "$CURRENT_BRANCH" = "master" ]; then
    echo -e "\n${YELLOW}▶ Estás en la rama 'master'. GitHub usa 'main' por defecto.${NC}"
    read -p "¿Quieres renombrar tu rama local a 'main'? (s/n): " RENAME_ANSWER
    if [[ "$RENAME_ANSWER" =~ ^[Ss]$ ]]; then
        git branch -m master main
        if [ $? -eq 0 ]; then
            CURRENT_BRANCH="main"
            echo -e "${GREEN}✓ Rama renombrada a 'main'.${NC}"
        else
            echo -e "${RED}✗ Error al renombrar la rama.${NC}"
        fi
    else
        echo -e "${YELLOW}Continuarás con la rama 'master'. Asegúrate de que el remoto también use ese nombre.${NC}"
    fi
elif [ "$CURRENT_BRANCH" != "main" ]; then
    echo -e "\n${YELLOW}▶ Tu rama actual es '$CURRENT_BRANCH'. GitHub suele usar 'main'.${NC}"
    read -p "¿Quieres renombrarla a 'main'? (s/n): " RENAME_OTHER
    if [[ "$RENAME_OTHER" =~ ^[Ss]$ ]]; then
        git branch -m "$CURRENT_BRANCH" main
        if [ $? -eq 0 ]; then
            CURRENT_BRANCH="main"
            echo -e "${GREEN}✓ Rama renombrada a 'main'.${NC}"
        else
            echo -e "${RED}✗ Error al renombrar.${NC}"
        fi
    fi
fi
pause

# 5. Configurar remote origin
echo -e "\n${YELLOW}▶ Verificando remote 'origin'...${NC}"
if git remote get-url origin >/dev/null 2>&1; then
    REMOTE_URL=$(git remote get-url origin)
    echo -e "${GREEN}✓ Remote origin ya configurado:${NC} $REMOTE_URL"
    read -p "¿Quieres cambiarlo? (s/n): " CHANGE_REMOTE
    if [[ "$CHANGE_REMOTE" =~ ^[Ss]$ ]]; then
        git remote remove origin
        read -p "Introduce la nueva URL del repositorio remoto: " REMOTE_URL
        git remote add origin "$REMOTE_URL"
        echo -e "${GREEN}✓ Remote origin actualizado.${NC}"
    fi
else
    echo -e "${RED}No hay remote 'origin' configurado.${NC}"
    read -p "Introduce la URL del repositorio remoto (HTTPS o SSH): " REMOTE_URL
    git remote add origin "$REMOTE_URL"
    if [ $? -eq 0 ]; then
        echo -e "${GREEN}✓ Remote origin agregado.${NC}"
    else
        echo -e "${RED}✗ Error al agregar remote.${NC}"
        exit 1
    fi
fi
pause

# 6. Hacer pull (integrar cambios remotos)
echo -e "\n${YELLOW}▶ ¿Quieres traer los cambios del remoto (pull) antes de hacer push?${NC}"
echo "   (Recomendado si el repositorio remoto ya tiene commits como README o .gitignore)"
read -p "¿Hacer pull? (s/n): " PULL_ANSWER
if [[ "$PULL_ANSWER" =~ ^[Ss]$ ]]; then
    echo -e "${YELLOW}Haciendo pull de la rama remota '$CURRENT_BRANCH'...${NC}"
    # Intentar pull normal
    if git pull origin "$CURRENT_BRANCH" 2>/dev/null; then
        echo -e "${GREEN}✓ Pull exitoso.${NC}"
    else
        echo -e "${YELLOW}El pull falló. Puede ser porque las historias no están relacionadas.${NC}"
        read -p "¿Quieres intentar con --allow-unrelated-histories? (s/n): " ALLOW_ANSWER
        if [[ "$ALLOW_ANSWER" =~ ^[Ss]$ ]]; then
            git pull origin "$CURRENT_BRANCH" --allow-unrelated-histories
            if [ $? -eq 0 ]; then
                echo -e "${GREEN}✓ Pull con --allow-unrelated-histories exitoso.${NC}"
            else
                echo -e "${RED}✗ Error en pull. Puede haber conflictos.${NC}"
                echo "Resuelve los conflictos manualmente y luego ejecuta el script de nuevo."
                exit 1
            fi
        else
            echo -e "${YELLOW}Omitiendo pull. Cuidado con los conflictos al hacer push.${NC}"
        fi
    fi
fi
pause

# 7. Agregar cambios y commit
echo -e "\n${YELLOW}▶ ¿Quieres agregar todos los archivos y hacer un commit?${NC}"
read -p "¿Hacer add y commit? (s/n): " COMMIT_ANSWER
if [[ "$COMMIT_ANSWER" =~ ^[Ss]$ ]]; then
    git add .
    echo -e "${GREEN}Archivos agregados al staging.${NC}"
    read -p "Mensaje para el commit: " COMMIT_MSG
    if [ -z "$COMMIT_MSG" ]; then
        COMMIT_MSG="Commit automático desde script"
    fi
    git commit -m "$COMMIT_MSG"
    if [ $? -eq 0 ]; then
        echo -e "${GREEN}✓ Commit realizado.${NC}"
    else
        echo -e "${YELLOW}No hay cambios para commitear o hubo un error.${NC}"
    fi
fi
pause

# 8. Hacer push
echo -e "\n${YELLOW}▶ Ahora vamos a hacer push.${NC}"
read -p "¿Quieres hacer push ahora? (s/n): " PUSH_ANSWER
if [[ "$PUSH_ANSWER" =~ ^[Ss]$ ]]; then
    # Intentar push normal
    if git push -u origin "$CURRENT_BRANCH" 2>/dev/null; then
        echo -e "${GREEN}✓ Push exitoso.${NC}"
    else
        echo -e "${RED}✗ El push falló.${NC}"
        echo "Posibles causas:"
        echo "  1) El remoto tiene cambios que no has integrado (necesitas pull primero)."
        echo "  2) La rama remota tiene otro nombre."
        echo "  3) Problemas de autenticación (token o SSH)."
        echo ""
        echo "Opciones:"
        echo "  1) Hacer pull primero y luego push (recomendado)"
        echo "  2) Forzar push (sobrescribir remoto) - PELIGROSO"
        echo "  3) Verificar y configurar remote"
        echo "  4) Cancelar"
        read -p "Elige una opción (1/2/3/4): " PUSH_OPTION
        case $PUSH_OPTION in
            1)
                echo -e "${YELLOW}Haciendo pull...${NC}"
                git pull origin "$CURRENT_BRANCH" --allow-unrelated-histories
                if [ $? -eq 0 ]; then
                    git push -u origin "$CURRENT_BRANCH"
                    if [ $? -eq 0 ]; then
                        echo -e "${GREEN}✓ Push exitoso después de pull.${NC}"
                    else
                        echo -e "${RED}✗ Error en push después de pull.${NC}"
                    fi
                else
                    echo -e "${RED}✗ Error en pull. Resuelve conflictos manualmente.${NC}"
                fi
                ;;
            2)
                echo -e "${RED}¡ADVERTENCIA! Forzar el push sobrescribirá los cambios en el remoto.${NC}"
                read -p "¿Estás seguro? (s/n): " FORCE_ANSWER
                if [[ "$FORCE_ANSWER" =~ ^[Ss]$ ]]; then
                    git push -f -u origin "$CURRENT_BRANCH"
                    if [ $? -eq 0 ]; then
                        echo -e "${GREEN}✓ Push forzado exitoso.${NC}"
                    else
                        echo -e "${RED}✗ Error en push forzado.${NC}"
                    fi
                fi
                ;;
            3)
                echo -e "${YELLOW}Configuración actual del remote:${NC}"
                git remote -v
                read -p "¿Quieres cambiar la URL del remote? (s/n): " CHANGE_URL
                if [[ "$CHANGE_URL" =~ ^[Ss]$ ]]; then
                    git remote remove origin
                    read -p "Introduce la nueva URL: " NEW_URL
                    git remote add origin "$NEW_URL"
                    echo -e "${GREEN}✓ Remote actualizado. Intenta push de nuevo.${NC}"
                fi
                ;;
            4)
                echo -e "${YELLOW}Push cancelado.${NC}"
                ;;
            *)
                echo -e "${RED}Opción no válida.${NC}"
                ;;
        esac
    fi
fi

echo -e "\n${GREEN}=== Proceso finalizado ===${NC}"
