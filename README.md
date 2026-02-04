# proyecto-diario-python

import streamlit as st
import pandas as pd

# Configuración de la página
st.set_page_config(page_title="Mi Tienda Online", layout="wide")

# --- BASE DE DATOS DE PRODUCTOS ---
if 'productos' not in st.session_state:
    st.session_state.productos = [
        {"id": 1, "nombre": "Biblia de Estudio", "precio": 25.0, "img": "📖"},
        {"id": 2, "nombre": "Diario de Notas", "precio": 10.0, "img": "📓"},
        {"id": 3, "nombre": "Set de Bolígrafos", "precio": 5.0, "img": "🖊️"},
        {"id": 4, "nombre": "Taza Personalizada", "precio": 12.0, "img": "☕"},
    ]

# --- LÓGICA DEL CARRITO ---
if 'carrito' not in st.session_state:
    st.session_state.carrito = []

def agregar_al_carrito(producto):
    st.session_state.carrito.append(producto)
    st.toast(f"✅ {producto['nombre']} añadido!")

# --- INTERFAZ DE USUARIO ---
st.title("🛒 Mi Tienda Virtual")
st.markdown("---")

# Layout de dos columnas: Productos y Carrito
col_prod, col_cart = st.columns([2, 1])

with col_prod:
    st.subheader("Catálogo de Productos")
    # Mostrar productos en una cuadrícula
    cols = st.columns(2)
    for idx, p in enumerate(st.session_state.productos):
        with cols[idx % 2]:
            st.info(f"{p['img']} **{p['nombre']}**")
            st.write(f"Precio: ${p['precio']}")
            if st.button(f"Añadir al carrito", key=f"btn_{p['id']}"):
                agregar_al_carrito(p)

with col_cart:
    st.subheader("Tu Carrito")
    if not st.session_state.carrito:
        st.write("El carrito está vacío 💨")
    else:
        df_carrito = pd.DataFrame(st.session_state.carrito)
        st.table(df_carrito[['nombre', 'precio']])
        
        total = df_carrito['precio'].sum()
        st.metric("Total a Pagar", f"${total}")
        
        if st.button("💳 Finalizar Compra"):
            st.success("¡Gracias por tu compra! Procesando pedido...")
            st.session_state.carrito = [] # Limpiar carrito
