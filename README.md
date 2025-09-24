# 🪙 Cotizador de Criptomonedas

Una aplicación web moderna y elegante para consultar precios en tiempo real de las principales criptomonedas del mercado. Desarrollada con React 19, TypeScript y las mejores prácticas de desarrollo frontend.

---

## 🚀 Características Principales

✨ **Consulta en Tiempo Real** - Obtén precios actualizados de las top 20 criptomonedas por capitalización de mercado  
🌍 **Múltiples Monedas** - Soporte para USD, EUR, MXN y GBP  
📊 **Información Detallada** - Precios máximos, mínimos del día y variación en 24h  
🎨 **Interfaz Moderna** - Diseño responsivo con animaciones suaves y spinner de carga  
🔒 **Validación Robusta** - Esquemas Zod para validación de datos de la API  
🏪 **Gestión de Estado Avanzada** - Implementado con Zustand para un manejo eficiente del estado  
⚡ **Alto Rendimiento** - Construido con Vite y React 19 para máxima velocidad  

---

## 🛠️ Tecnologías Utilizadas

### Frontend Framework
- **React** `^19.1.1` - Biblioteca principal de interfaz de usuario
- **TypeScript** `~5.8.3` - Superset tipado de JavaScript
- **Vite** `^7.1.2` - Build tool y servidor de desarrollo ultrarrápido

### Gestión de Estado y Datos
- **Zustand** `^5.0.8` - Librería ligera para manejo de estado global
- **Axios** `^1.11.0` - Cliente HTTP para comunicación con APIs
- **Zod** `^4.1.3` - Validación y parsing de esquemas TypeScript-first

### Herramientas de Desarrollo
- **ESLint** `^9.33.0` - Linter para mantener calidad de código
- **@vitejs/plugin-react-swc** `^4.0.0` - Plugin SWC para Fast Refresh optimizado
- **TypeScript ESLint** `^8.39.1` - Reglas específicas de ESLint para TypeScript

### API Externa
- **CryptoCompare API** - Datos financieros en tiempo real de criptomonedas

---

## 📦 Instalación y Configuración

### Prerrequisitos
```bash
Node.js >= 18.0.0
npm >= 9.0.0
```

### 1️⃣ Clonar el Repositorio
```bash
git clone https://github.com/Chivan9406/CryptoCurrencies-React.git
cd CryptoCurrencies-React
```

### 2️⃣ Instalar Dependencias
```bash
npm install
```

### 3️⃣ Ejecutar en Desarrollo
```bash
npm run dev
```

La aplicación estará disponible en `http://localhost:5173`

### 4️⃣ Construir para Producción
```bash
npm run build
```

### 5️⃣ Previsualizar Build de Producción
```bash
npm run preview
```

---

## 🏗️ Estructura del Proyecto

```
CryptoCurrencies-React/
├── 📁 src/
│   ├── 📁 components/          # Componentes reutilizables
│   │   ├── CryptoPriceDisplay.tsx    # Muestra cotización y detalles
│   │   ├── CryptoSearchForm.tsx      # Formulario de búsqueda
│   │   ├── ErrorMessage.tsx          # Componente de mensajes de error
│   │   └── Spinner.tsx               # Indicador de carga
│   ├── 📁 data/                # Datos estáticos y configuración
│   │   └── index.ts                  # Lista de monedas disponibles
│   ├── 📁 schema/              # Esquemas de validación Zod
│   │   └── crypto-schema.ts          # Validaciones para datos de crypto
│   ├── 📁 services/            # Servicios de comunicación con APIs
│   │   └── CryptoService.ts          # Cliente para CryptoCompare API
│   ├── 📁 store/               # Gestión de estado global
│   │   └── store.ts                  # Store principal con Zustand
│   ├── 📁 types/               # Definiciones de tipos TypeScript
│   │   └── index.ts                  # Tipos derivados de esquemas Zod
│   ├── App.tsx                 # Componente principal de la aplicación
│   ├── main.tsx                # Punto de entrada de React
│   └── index.css               # Estilos globales
├── 📁 public/                  # Recursos estáticos
├── package.json                # Configuración del proyecto y dependencias
├── vite.config.ts              # Configuración de Vite
├── tsconfig.json               # Configuración de TypeScript
└── eslint.config.js            # Configuración de ESLint
```

---

## 🎯 Funcionalidades Principales

### 🔍 Búsqueda de Criptomonedas
- Lista de las **top 20 criptomonedas** por capitalización de mercado
- Selección de moneda de cotización (USD, EUR, MXN, GBP)
- Validación de formularios con mensajes de error personalizados

### 📈 Visualización de Datos
- **Precio actual** de la criptomoneda seleccionada
- **Precio máximo y mínimo** del día
- **Porcentaje de cambio** en las últimas 24 horas
- **Última actualización** de los datos
- **Imagen representativa** de cada criptomoneda

### ⚡ Estados de Carga
- Spinner animado durante las consultas a la API
- Manejo de estados de carga y error
- Actualización automática de la lista de criptomonedas al cargar la app

---

## 🎨 Componentes Principales

### `CryptoSearchForm`
Formulario principal que permite seleccionar la criptomoneda y moneda de cotización:

```tsx
// Manejo de estado local para el par de monedas
const [pair, setPair] = useState<Pair>({
  currency: '',
  cryptoCurrency: ''
})

// Validación y envío del formulario
const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
  e.preventDefault()
  if (Object.values(pair).includes('')) {
    setError('Todos los campos son obligatorios')
    return
  }
  fetchData(pair)
}
```

### `CryptoPriceDisplay`
Componente que muestra la información detallada de la cotización:

```tsx
// Memo para optimizar renders
const hasResult = useMemo(() => 
  !Object.values(result).includes(''), [result]
)

// Renderizado condicional con estado de carga
return (
  <div className="result-wrapper">
    {loading ? <Spinner /> : hasResult && (
      <div className="result">
        <img src={`https://cryptocompare.com/${result.IMAGEURL}`} />
        <div>
          <p>El precio es de: <span>{result.PRICE}</span></p>
          {/* ... más información */}
        </div>
      </div>
    )}
  </div>
)
```

---

## 📊 Gestión de Estado con Zustand

El estado global de la aplicación se maneja con **Zustand**, proporcionando una solución ligera y eficiente:

```tsx
type CryptoStore = {
  cryptoCurrencies: CryptoCurrency[]  // Lista de criptomonedas disponibles
  result: CryptoPrice                 // Resultado de la cotización actual
  loading: boolean                    // Estado de carga
  fetchCryptos: () => Promise<void>   // Obtener lista de cryptos
  fetchData: (pair: Pair) => Promise<void>  // Obtener cotización específica
}

export const useCryptoStore = create<CryptoStore>()(
  devtools((set) => ({
    // Estado inicial y acciones...
  }))
)
```

### Características del Store:
- **DevTools Integration** - Debugging con Redux DevTools
- **Async Actions** - Manejo de operaciones asíncronas
- **Type Safety** - Completamente tipado con TypeScript
- **Minimal Boilerplate** - Menos código que Redux tradicional

---

## 🔧 Scripts Disponibles

| Script | Descripción | Comando |
|--------|-------------|---------|
| **Desarrollo** | Inicia el servidor de desarrollo con HMR | `npm run dev` |
| **Build** | Compila TypeScript y construye para producción | `npm run build` |
| **Preview** | Previsualiza el build de producción localmente | `npm run preview` |
| **Lint** | Ejecuta ESLint para análisis de código | `npm run lint` |

---

## 🌟 Características Técnicas Destacadas

### 🔒 **Validación de Datos con Zod**
```tsx
export const CryptoPriceSchema = z.object({
  IMAGEURL: z.string(),
  PRICE: z.string(),
  HIGHDAY: z.string(),
  LOWDAY: z.string(),
  CHANGEPCT24HOUR: z.string(),
  LASTUPDATE: z.string()
})

// Parsing seguro con validación
const result = CryptoPriceSchema.safeParse(apiData)
if (result.success) {
  return result.data  // Datos tipados y validados
}
```

### ⚡ **Optimizaciones de Performance**
- **React 19** con las últimas optimizaciones
- **useMemo** para evitar recálculos innecesarios
- **Lazy Loading** de componentes cuando sea necesario
- **Vite** para builds ultrarrápidos y HMR instantáneo

### 🏛️ **Arquitectura Limpia**
- **Separación de responsabilidades** clara
- **Servicios dedicados** para comunicación con APIs
- **Tipos centralizados** generados desde esquemas
- **Componentes reutilizables** y bien estructurados

### 🔄 **Manejo de Estados Asíncronos**
```tsx
fetchData: async (pair) => {
  set(() => ({ loading: true }))        // Inicia loading
  const result = await fetchCurrentCryptoPrice(pair)
  set(() => ({ result, loading: false })) // Actualiza resultado
}
```

---

## 📄 Licencia

Este proyecto está bajo la **Licencia MIT**. Consulta el archivo `LICENSE` para más detalles.

---

## 👨‍💻 Autor

**Ivan Chivapchich**  
🔗 **GitHub:** [@Chivan9406](https://github.com/Chivan9406)

---

<div align="center">

**⭐ Si este proyecto te fue útil, considera darle una estrella en GitHub ⭐**

*Desarrollado con ❤️ usando React 19, TypeScript y las mejores prácticas de desarrollo moderno*

</div>
