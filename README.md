# Sentinela GNR - Android APK

📱 **App Android shell para a PWA Sentinela GNR**

Este projeto cria um APK Android que funciona como "casca" (wrapper) para a tua aplicação web Sentinela GNR. A lógica de negócio fica toda na PWA (na cloud), enquanto o APK apenas abre um WebView com permissões para câmera, microfone e upload de ficheiros.

---

## 🎯 Características

✅ **WebView Full-Screen** - A tua PWA ocupa todo o ecrã
✅ **Permissões Runtime** - Câmera, Microfone, Imagens (Android 13+)
✅ **Upload de Ficheiros** - Suporta `<input type="file">` com acesso a câmera/galeria
✅ **getUserMedia Bridge** - A PWA pode usar câmera e microfone via Web APIs
✅ **Navegação com Back** - Botão voltar funciona dentro do WebView
✅ **Ícone e Nome** - Aparece no launcher como "Sentinela GNR"
✅ **Sem Lógica de Negócio** - Toda a app fica na PWA

---

## 📦 Requisitos

- **Android Studio** (2023.1+ recomendado)
- **JDK 11** ou superior
- **SDK Android** 23+ (minSdk) e 34 (targetSdk)
- **Kotlin 1.9+**

---

## 🚀 Como Usar (Passo a Passo)

### **1️⃣ Clone o Repositório**

```bash
git clone https://github.com/safe81/sentinela-gnr-android.git
cd sentinela-gnr-android
```

### **2️⃣ Abre no Android Studio**

1. Abre **Android Studio**
2. Clica em **File → Open**
3. Seleciona a pasta `sentinela-gnr-android`
4. Aguarda o Gradle sincronizar (pode demorar alguns minutos)

### **3️⃣ Configura o URL da Tua PWA**

📍 **Ficheiro:** `app/src/main/java/com/sentinelagnr/MainActivity.kt`

**Linha 26:**
```kotlin
private val SENTINELA_URL = "https://SENTINELA_URL_AQUI"
```

**Substitui** `https://SENTINELA_URL_AQUI` pelo URL da tua PWA Sentinela GNR.

**Exemplo:**
```kotlin
private val SENTINELA_URL = "https://sentinela-gnr.netlify.app"
```

### **4️⃣ Testa no Emulador ou Dispositivo**

1. Clica em **Run → Run 'app'** (ou pressiona `Shift+F10`)
2. Escolhe um emulador Android ou liga um dispositivo físico via USB
3. A app deve abrir e carregar a tua PWA

### **5️⃣ Gera o APK Assinado para Distribuição**

#### **a) Cria uma Keystore (primeira vez)**

1. No Android Studio: **Build → Generate Signed Bundle / APK**
2. Escolhe **APK** e clica em **Next**
3. Clica em **Create new...** para criar uma nova keystore
4. Preenche os dados:
   - **Key store path**: Escolhe onde guardar (ex: `sentinela-gnr-keystore.jks`)
   - **Password**: Define uma password forte
   - **Key alias**: `sentinela-gnr`
   - **Key password**: Define outra password
   - **Validity (years)**: 25
   - **First and Last Name**: O teu nome ou da organização
5. Clica em **OK**

#### **b) Assina e Gera o APK**

1. Seleciona a keystore que acabaste de criar
2. Insere as passwords
3. Marca **release** como Build Variant
4. Marca as duas **Signature Versions** (V1 e V2)
5. Clica em **Finish**

#### **c) Encontra o APK**

O APK assinado fica em:
```
app/release/app-release.apk
```

---

## 📂 Estrutura do Projeto

```
sentinela-gnr-android/
├── app/
│   ├── src/
│   │   └── main/
│   │       ├── AndroidManifest.xml      # Permissões e configurações
│   │       ├── java/com/sentinelagnr/
│   │       │   └── MainActivity.kt      # ⚠️ AQUI MUDAS O URL
│   │       └── res/
│   │           ├── layout/
│   │           │   └── activity_main.xml # Layout do WebView
│   │           └── values/
│   │               └── strings.xml       # Nome da app
│   └── build.gradle                      # Dependências do módulo
├── build.gradle                          # Gradle raiz
└── settings.gradle                       # Config do projeto
```

---

## 🔧 Personalização

### **Mudar o Nome da App**

📍 **Ficheiro:** `app/src/main/res/values/strings.xml`

```xml
<string name="app_name">Sentinela GNR</string>
```

### **Mudar o Ícone**

1. Substitui os ficheiros em `app/src/main/res/mipmap-*/`
2. Ou usa **File → New → Image Asset** no Android Studio

### **Mudar o Package Name**

📍 **Ficheiros:**
- `app/build.gradle` → `applicationId`
- `app/src/main/AndroidManifest.xml` → `package`
- Refactoriza o package em `MainActivity.kt`

---

## 🛠️ Troubleshooting

### **Problema: "App not installed"**

- **Causa**: Keystore diferente de uma versão anterior
- **Solução**: Desinstala a versão antiga primeiro ou usa a mesma keystore

### **Problema: Câmera não abre**

- **Verifica**: As permissões no `AndroidManifest.xml`
- **Verifica**: O `WebChromeClient` tem `onPermissionRequest`
- **Testa**: Dá permissões manualmente nas Settings do Android

### **Problema: Upload de ficheiros não funciona**

- **Verifica**: O `onShowFileChooser` está implementado
- **Verifica**: A PWA usa `<input type="file">`

### **Problema: Gradle sync failed**

- **Solução**: **File → Invalidate Caches → Invalidate and Restart**
- Ou apaga a pasta `.gradle` e sincroniza de novo

---

## 📱 Distribuição do APK

### **Opção 1: Distribuição Direta (Fora da Play Store)**

1. Envia o `app-release.apk` por email/WhatsApp/Dropbox
2. Os utilizadores precisam ativar **"Instalar apps de fontes desconhecidas"**
3. Instruções: Settings → Security → Unknown Sources

### **Opção 2: Google Play Store**

1. Cria uma conta de **Google Play Developer** (25 USD one-time)
2. Usa **App Bundle** (.aab) em vez de APK:
   - **Build → Generate Signed Bundle / APK → Android App Bundle**
3. Faz upload na Play Console
4. Preenche os detalhes da app e publica

---

## 🔐 Segurança

- ✅ A app **só carrega** o URL que definires em `SENTINELA_URL`
- ✅ Navegação para outros domínios é **bloqueada**
- ✅ Permissões são pedidas ao utilizador (runtime)
- ⚠️ **IMPORTANTE**: Guarda a **keystore** num local seguro! Sem ela não podes atualizar a app.

---

## 📄 Licença

Este projeto é open-source. Usa à vontade para a tua Sentinela GNR! 🚁

---

## 🤝 Suporte

Problemas? Abre uma **Issue** no GitHub ou contacta o desenvolvedor.

---

**Feito com ❤️ para a GNR** 🇵🇹
