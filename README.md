import { useMemo, useState } from "react";

export default function BunkerBebidasWeb() {
  const products = [
    {
      id: 1,
      name: "Combo Fernet",
      price: 18900,
      desc: "Fernet + 2 colas 2.25L",
      badge: "Selección",
    },
    {
      id: 2,
      name: "Promo Gin",
      price: 37100,
      desc: "Tanqueray + 2 tónicas",
      badge: "Destacado",
    },
    {
      id: 3,
      name: "Pack Cervezas",
      price: 12500,
      desc: "6 latas surtidas frías",
      badge: "Frías",
    },
    {
      id: 4,
      name: "Vodka + Energética",
      price: 24800,
      desc: "Ideal para previa",
      badge: "Nuevo",
    },
  ];

  const categories = [
    "Whiskys",
    "Vinos",
    "Cervezas",
    "Gin",
    "Espumantes",
    "Promociones",
  ];

  const [cart, setCart] = useState([]);
  const [deliveryType, setDeliveryType] = useState("retiro");
  const [customer, setCustomer] = useState({
    name: "",
    phone: "",
    address: "",
    notes: "",
  });

  const formatPrice = (value) =>
    new Intl.NumberFormat("es-AR", {
      style: "currency",
      currency: "ARS",
      maximumFractionDigits: 0,
    }).format(value);

  const addToCart = (product) => {
    setCart((prev) => {
      const existing = prev.find((item) => item.id === product.id);
      if (existing) {
        return prev.map((item) =>
          item.id === product.id ? { ...item, qty: item.qty + 1 } : item
        );
      }
      return [...prev, { ...product, qty: 1 }];
    });
  };

  const changeQty = (id, delta) => {
    setCart((prev) =>
      prev
        .map((item) =>
          item.id === id ? { ...item, qty: Math.max(0, item.qty + delta) } : item
        )
        .filter((item) => item.qty > 0)
    );
  };

  const subtotal = useMemo(
    () => cart.reduce((acc, item) => acc + item.price * item.qty, 0),
    [cart]
  );

  const deliveryFee = deliveryType === "envio" ? 2500 : 0;
  const total = subtotal + deliveryFee;

  const whatsappMessage = useMemo(() => {
    const lines = [
      "Hola Bunker de Bebidas, quiero hacer este pedido:",
      "",
    ];

    if (cart.length === 0) {
      lines.push("- Carrito vacío");
    } else {
      cart.forEach((item) => {
        lines.push(
          `- ${item.name} x${item.qty} = ${formatPrice(item.price * item.qty)}`
        );
      });
    }

    lines.push("");
    lines.push(`Forma de entrega: ${deliveryType === "envio" ? "Envío" : "Retiro por el local"}`);
    lines.push(`Subtotal: ${formatPrice(subtotal)}`);
    lines.push(`Costo de ${deliveryType === "envio" ? "envío" : "retiro"}: ${formatPrice(deliveryFee)}`);
    lines.push(`Total: ${formatPrice(total)}`);
    lines.push("");
    lines.push(`Nombre: ${customer.name || "-"}`);
    lines.push(`Teléfono: ${customer.phone || "-"}`);
    if (deliveryType === "envio") {
      lines.push(`Dirección: ${customer.address || "-"}`);
    }
    lines.push(`Notas: ${customer.notes || "-"}`);
    lines.push("");
    lines.push("Comprobante: enviar por este chat una vez realizado el pago.");

    return encodeURIComponent(lines.join("\n"));
  }, [cart, customer, deliveryFee, deliveryType, subtotal, total]);

  return (
    <div className="min-h-screen bg-[#f5f1ec] text-[#8e1623]">
      <header className="sticky top-0 z-50 border-b border-[#8e1623]/15 bg-[#f5f1ec]/95 backdrop-blur">
        <div className="mx-auto flex max-w-7xl items-center justify-between px-6 py-4">
          <div>
            <p className="text-xs uppercase tracking-[0.35em] text-[#8e1623]/65">Vinoteca · Promos · Pedidos</p>
            <h1 className="mt-1 text-3xl font-black tracking-wide md:text-4xl" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>
              BUNKER DE BEBIDAS
            </h1>
          </div>
          <a
            href="#carrito"
            className="rounded-full border border-[#8e1623] px-5 py-2 text-sm font-bold text-[#8e1623] transition hover:bg-[#8e1623] hover:text-[#f5f1ec]"
          >
            CARRITO ({cart.reduce((acc, item) => acc + item.qty, 0)})
          </a>
        </div>
      </header>

      <section className="relative overflow-hidden">
        <div className="mx-auto grid max-w-7xl gap-12 px-6 py-16 md:grid-cols-2 md:py-24">
          <div className="flex flex-col justify-center">
            <span className="mb-5 w-fit border-b border-[#8e1623]/30 pb-2 text-xs uppercase tracking-[0.4em] text-[#8e1623]/70">
              Estilo de marca · Elegancia · Venta directa
            </span>
            <h2
              className="max-w-xl text-5xl font-black leading-[0.95] md:text-7xl"
              style={{ fontFamily: "Georgia, Times New Roman, serif" }}
            >
              BUNKER <span className="inline-block text-[#8e1623]/70">DE</span> BEBIDAS
            </h2>
            <p className="mt-6 max-w-xl text-base leading-7 text-[#8e1623]/75 md:text-lg">
              Sumamos carrito, total automático y envío o retiro para que el cliente arme su pedido más fácil.
            </p>

            <div className="mt-8 flex flex-wrap gap-4">
              <a
                href="#productos"
                className="rounded-full bg-[#8e1623] px-6 py-3 text-sm font-bold tracking-wide text-[#f5f1ec] transition hover:scale-105"
              >
                VER CATÁLOGO
              </a>
              <a
                href="#carrito"
                className="rounded-full border border-[#8e1623]/30 px-6 py-3 text-sm font-bold tracking-wide text-[#8e1623] transition hover:bg-[#8e1623]/5"
              >
                IR AL CARRITO
              </a>
            </div>
          </div>

          <div className="flex items-center">
            <div className="w-full rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-7 shadow-[0_20px_60px_rgba(142,22,35,0.08)]">
              <div className="border-b border-[#8e1623]/15 pb-5">
                <p className="text-xs uppercase tracking-[0.3em] text-[#8e1623]/60">Compra rápida</p>
                <h3 className="mt-3 text-4xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>
                  ARMÁ TU PEDIDO
                </h3>
                <p className="mt-3 text-[#8e1623]/70">Elegí productos, mirá el total y mandanos el pedido por WhatsApp con todos los datos.</p>
              </div>

              <div className="grid gap-4 pt-5 md:grid-cols-2">
                <div className="rounded-[1.5rem] border border-[#8e1623]/15 bg-[#f5f1ec] p-5">
                  <p className="text-xs uppercase tracking-[0.25em] text-[#8e1623]/60">Retiro</p>
                  <div className="mt-2 text-lg font-bold">Sin costo extra</div>
                </div>
                <div className="rounded-[1.5rem] border border-[#8e1623]/15 bg-[#f5f1ec] p-5">
                  <p className="text-xs uppercase tracking-[0.25em] text-[#8e1623]/60">Envío</p>
                  <div className="mt-2 text-lg font-bold">Sumado al total</div>
                </div>
              </div>

              <div className="mt-4 rounded-[1.5rem] border border-[#8e1623]/15 bg-[#f5f1ec] p-5">
                <p className="text-xs uppercase tracking-[0.25em] text-[#8e1623]/60">Medios de pago</p>
                <div className="mt-3 flex flex-wrap gap-2 text-sm font-semibold text-[#8e1623]/85">
                  <span className="rounded-full border border-[#8e1623]/20 px-4 py-2">Efectivo</span>
                  <span className="rounded-full border border-[#8e1623]/20 px-4 py-2">Transferencia</span>
                  <span className="rounded-full border border-[#8e1623]/20 px-4 py-2">Tarjetas</span>
                  <span className="rounded-full border border-[#8e1623]/20 px-4 py-2">QR</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section className="mx-auto max-w-7xl px-6 py-2">
        <div className="grid gap-6 md:grid-cols-3">
          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-6 shadow-[0_12px_40px_rgba(142,22,35,0.05)]">
            <p className="text-xs uppercase tracking-[0.3em] text-[#8e1623]/55">WhatsApp</p>
            <h4 className="mt-3 text-2xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>3454 15-8692</h4>
            <p className="mt-3 text-[#8e1623]/70">Pedidos y consultas directas.</p>
          </div>
          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-6 shadow-[0_12px_40px_rgba(142,22,35,0.05)]">
            <p className="text-xs uppercase tracking-[0.3em] text-[#8e1623]/55">Ubicación</p>
            <h4 className="mt-3 text-2xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>Sarmiento 700</h4>
            <p className="mt-3 text-[#8e1623]/70">Entre Ríos, Concordia.</p>
          </div>
          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-6 shadow-[0_12px_40px_rgba(142,22,35,0.05)]">
            <p className="text-xs uppercase tracking-[0.3em] text-[#8e1623]/55">Horarios</p>
            <p className="mt-3 text-sm leading-7 text-[#8e1623]/80">
              Mañana: 9:00 a 13:00<br />
              Lunes a jueves: 17:00 a 22:00<br />
              Viernes y sábados: 17:00 a 23:00
            </p>
          </div>
        </div>
      </section>

      <section className="mx-auto max-w-7xl px-6 py-6">
        <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/50 p-6 shadow-[0_12px_40px_rgba(142,22,35,0.05)]">
          <p className="mb-4 text-xs uppercase tracking-[0.35em] text-[#8e1623]/55">Líneas destacadas</p>
          <div className="flex flex-wrap gap-3">
            {categories.map((category) => (
              <span
                key={category}
                className="rounded-full border border-[#8e1623]/20 px-4 py-2 text-sm font-semibold text-[#8e1623]/85"
              >
                {category}
              </span>
            ))}
          </div>
        </div>
      </section>

      <section id="productos" className="mx-auto max-w-7xl px-6 py-14">
        <div className="mb-8 flex items-end justify-between gap-4">
          <div>
            <p className="text-xs uppercase tracking-[0.35em] text-[#8e1623]/55">Catálogo</p>
            <h3 className="mt-2 text-4xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>
              PRODUCTOS DESTACADOS
            </h3>
          </div>
          <p className="text-sm font-semibold text-[#8e1623]/55">Agregalos al carrito desde acá</p>
        </div>

        <div className="grid gap-6 md:grid-cols-2 xl:grid-cols-4">
          {products.map((product) => (
            <div
              key={product.id}
              className="group rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-5 shadow-[0_14px_40px_rgba(142,22,35,0.06)] transition hover:-translate-y-1"
            >
              <div className="mb-5 flex items-center justify-between">
                <span className="rounded-full border border-[#8e1623]/20 bg-[#8e1623]/5 px-3 py-1 text-xs font-bold uppercase tracking-[0.18em] text-[#8e1623]">
                  {product.badge}
                </span>
                <span className="text-xs uppercase tracking-[0.15em] text-[#8e1623]/40">Stock</span>
              </div>
              <div className="rounded-[1.5rem] border border-dashed border-[#8e1623]/20 bg-[#f5f1ec] p-8 text-center text-sm font-semibold uppercase tracking-[0.2em] text-[#8e1623]/35">
                Imagen del producto
              </div>
              <h4 className="mt-5 text-2xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>{product.name}</h4>
              <p className="mt-2 text-sm leading-6 text-[#8e1623]/70">{product.desc}</p>
              <div className="mt-5 text-2xl font-black">{formatPrice(product.price)}</div>
              <button
                onClick={() => addToCart(product)}
                className="mt-5 inline-flex rounded-full bg-[#8e1623] px-4 py-2 text-sm font-bold tracking-wide text-[#f5f1ec] transition hover:scale-105"
              >
                AGREGAR AL CARRITO
              </button>
            </div>
          ))}
        </div>
      </section>

      <section id="carrito" className="mx-auto max-w-7xl px-6 py-6">
        <div className="grid gap-6 lg:grid-cols-[1.2fr_0.8fr]">
          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-6 shadow-[0_14px_40px_rgba(142,22,35,0.06)]">
            <p className="text-xs uppercase tracking-[0.35em] text-[#8e1623]/55">Carrito</p>
            <h3 className="mt-2 text-4xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>
              TU PEDIDO
            </h3>

            <div className="mt-6 space-y-4">
              {cart.length === 0 ? (
                <div className="rounded-[1.5rem] border border-dashed border-[#8e1623]/20 bg-[#f5f1ec] p-6 text-[#8e1623]/60">
                  Todavía no agregaste productos.
                </div>
              ) : (
                cart.map((item) => (
                  <div key={item.id} className="rounded-[1.5rem] border border-[#8e1623]/15 bg-[#f5f1ec] p-4">
                    <div className="flex flex-wrap items-center justify-between gap-4">
                      <div>
                        <h4 className="text-xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>{item.name}</h4>
                        <p className="mt-1 text-sm text-[#8e1623]/70">{formatPrice(item.price)} c/u</p>
                      </div>
                      <div className="flex items-center gap-3">
                        <button
                          onClick={() => changeQty(item.id, -1)}
                          className="h-9 w-9 rounded-full border border-[#8e1623]/20 text-lg font-bold"
                        >
                          -
                        </button>
                        <span className="min-w-6 text-center font-bold">{item.qty}</span>
                        <button
                          onClick={() => changeQty(item.id, 1)}
                          className="h-9 w-9 rounded-full border border-[#8e1623]/20 text-lg font-bold"
                        >
                          +
                        </button>
                      </div>
                    </div>
                    <div className="mt-3 text-right text-lg font-black">{formatPrice(item.price * item.qty)}</div>
                  </div>
                ))
              )}
            </div>
          </div>

          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-[#8e1623] p-6 text-[#f5f1ec] shadow-[0_14px_40px_rgba(142,22,35,0.14)]">
            <p className="text-xs uppercase tracking-[0.35em] text-[#f5f1ec]/65">Finalizar compra</p>
            <h3 className="mt-2 text-3xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>
              DATOS DEL CLIENTE
            </h3>

            <div className="mt-5 grid gap-3">
              <input
                value={customer.name}
                onChange={(e) => setCustomer({ ...customer, name: e.target.value })}
                placeholder="Nombre y apellido"
                className="rounded-2xl border border-white/15 bg-white/10 px-4 py-3 text-white placeholder:text-white/60 outline-none"
              />
              <input
                value={customer.phone}
                onChange={(e) => setCustomer({ ...customer, phone: e.target.value })}
                placeholder="Teléfono"
                className="rounded-2xl border border-white/15 bg-white/10 px-4 py-3 text-white placeholder:text-white/60 outline-none"
              />

              <div className="mt-2 grid grid-cols-2 gap-3">
                <button
                  onClick={() => setDeliveryType("retiro")}
                  className={`rounded-2xl px-4 py-3 text-sm font-bold transition ${
                    deliveryType === "retiro"
                      ? "bg-[#f5f1ec] text-[#8e1623]"
                      : "border border-white/20 bg-white/10 text-white"
                  }`}
                >
                  RETIRO POR EL LOCAL
                </button>
                <button
                  onClick={() => setDeliveryType("envio")}
                  className={`rounded-2xl px-4 py-3 text-sm font-bold transition ${
                    deliveryType === "envio"
                      ? "bg-[#f5f1ec] text-[#8e1623]"
                      : "border border-white/20 bg-white/10 text-white"
                  }`}
                >
                  ME LO ENVÍAN
                </button>
              </div>

              {deliveryType === "envio" && (
                <input
                  value={customer.address}
                  onChange={(e) => setCustomer({ ...customer, address: e.target.value })}
                  placeholder="Dirección de entrega"
                  className="rounded-2xl border border-white/15 bg-white/10 px-4 py-3 text-white placeholder:text-white/60 outline-none"
                />
              )}

              <textarea
                value={customer.notes}
                onChange={(e) => setCustomer({ ...customer, notes: e.target.value })}
                placeholder="Notas del pedido"
                rows={4}
                className="rounded-2xl border border-white/15 bg-white/10 px-4 py-3 text-white placeholder:text-white/60 outline-none"
              />
            </div>

            <div className="mt-6 rounded-[1.5rem] bg-white/10 p-4">
              <div className="flex items-center justify-between text-sm">
                <span>Subtotal</span>
                <span>{formatPrice(subtotal)}</span>
              </div>
              <div className="mt-2 flex items-center justify-between text-sm">
                <span>{deliveryType === "envio" ? "Envío" : "Retiro"}</span>
                <span>{formatPrice(deliveryFee)}</span>
              </div>
              <div className="mt-4 flex items-center justify-between border-t border-white/15 pt-4 text-lg font-black">
                <span>Total</span>
                <span>{formatPrice(total)}</span>
              </div>
            </div>

            <div className="mt-6 grid gap-3">
              <a
                href={`https://wa.me/5493454158692?text=${whatsappMessage}`}
                className="rounded-full bg-[#f5f1ec] px-5 py-3 text-center text-sm font-bold tracking-wide text-[#8e1623] transition hover:scale-[1.02]"
              >
                ENVIAR PEDIDO POR WHATSAPP
              </a>
              <button
                className="rounded-full border border-[#f5f1ec]/30 px-5 py-3 text-sm font-bold tracking-wide text-[#f5f1ec] transition hover:bg-white/10"
              >
                PAGAR ONLINE
              </button>
            </div>

            <p className="mt-4 text-xs leading-6 text-[#f5f1ec]/75">
              El botón de pago online queda preparado visualmente. Para cobrar ahí mismo con comprobante automático hay que conectarlo después con una pasarela de pago.
            </p>
          </div>
        </div>
      </section>

      <section id="ofertas" className="mx-auto max-w-7xl px-6 py-6">
        <div className="grid gap-6 md:grid-cols-3">
          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-6">
            <p className="text-xs uppercase tracking-[0.3em] text-[#8e1623]/55">Promo 01</p>
            <h4 className="mt-3 text-3xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>Combos del finde</h4>
            <p className="mt-3 leading-7 text-[#8e1623]/70">Promociones especiales para viernes, sábado y domingo con una estética más premium.</p>
          </div>
          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-6">
            <p className="text-xs uppercase tracking-[0.3em] text-[#8e1623]/55">Promo 02</p>
            <h4 className="mt-3 text-3xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>Entrega rápida</h4>
            <p className="mt-3 leading-7 text-[#8e1623]/70">Tomá pedidos por WhatsApp y mostrá una presencia más prolija y más fuerte de marca.</p>
          </div>
          <div className="rounded-[2rem] border border-[#8e1623]/15 bg-white/60 p-6">
            <p className="text-xs uppercase tracking-[0.3em] text-[#8e1623]/55">Promo 03</p>
            <h4 className="mt-3 text-3xl font-black" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>Pagos simples</h4>
            <p className="mt-3 leading-7 text-[#8e1623]/70">Podés sumar alias, QR, ubicación y toda la info útil dentro de la misma web.</p>
          </div>
        </div>
      </section>

      <section className="mx-auto max-w-7xl px-6 py-16">
        <div className="rounded-[2.2rem] border border-[#8e1623]/15 bg-[#8e1623] p-8 text-center text-[#f5f1ec] shadow-[0_20px_60px_rgba(142,22,35,0.15)]">
          <p className="text-xs uppercase tracking-[0.35em] text-[#f5f1ec]/70">Contacto</p>
          <h3 className="mt-3 text-4xl font-black md:text-5xl" style={{ fontFamily: "Georgia, Times New Roman, serif" }}>
            HACÉ TU PEDIDO
          </h3>
          <p className="mx-auto mt-4 max-w-2xl text-[#f5f1ec]/80">
            Sarmiento 700, Concordia, Entre Ríos · Pedidos al 3454 15-8692 · Ahora con carrito y total automático.
          </p>
          <div className="mt-8 flex flex-wrap items-center justify-center gap-4">
            <a
              href={`https://wa.me/5493454158692?text=${whatsappMessage}`}
              className="rounded-full bg-[#f5f1ec] px-6 py-3 text-sm font-bold tracking-wide text-[#8e1623] transition hover:scale-105"
            >
              WHATSAPP
            </a>
            <a
              href="https://maps.google.com/?q=Sarmiento+700+Concordia+Entre+Rios"
              className="rounded-full border border-[#f5f1ec]/35 px-6 py-3 text-sm font-bold tracking-wide text-[#f5f1ec] transition hover:bg-white/10"
            >
              UBICACIÓN
            </a>
          </div>
        </div>
      </section>

      <footer className="border-t border-[#8e1623]/10 px-6 py-8 text-center text-sm font-semibold tracking-wide text-[#8e1623]/50">
        © 2026 BUNKER DE BEBIDAS · Catálogo · Promociones · Pedidos
        <div className="mt-2 flex flex-col items-center gap-2 text-xs">
          <span>Aceptamos todas las tarjetas bancarias</span>
          <div className="flex items-center gap-3 text-lg">
            <span>💳</span>
            <span className="font-bold">VISA</span>
            <span className="font-bold">Mastercard</span>
            <span className="font-bold">AMEX</span>
            <span className="font-bold">Credicoop</span>
          </div>
        </div>
      </footer>
    </div>
  );
}
