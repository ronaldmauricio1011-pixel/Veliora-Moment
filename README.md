# Veliora-Moment
fotografia para tus eventos especiales y para eventos
import { useState, useEffect } from "react";

export default function PortfolioPremium() {
  const [categorias, setCategorias] = useState([]);
  const [nuevaCategoria, setNuevaCategoria] = useState("");
  const [reserva, setReserva] = useState({
    nombre: "",
    email: "",
    fecha: "",
    servicio: ""
  });

  // Guardado persistente en navegador
  useEffect(() => {
    const data = localStorage.getItem("portfolioData");
    if (data) setCategorias(JSON.parse(data));
  }, []);

  useEffect(() => {
    localStorage.setItem("portfolioData", JSON.stringify(categorias));
  }, [categorias]);

  const agregarCategoria = () => {
    if (!nuevaCategoria) return;
    setCategorias([
      ...categorias,
      {
        nombre: nuevaCategoria,
        descripcion: "",
        precio: "",
        fotos: [],
        videos: []
      }
    ]);
    setNuevaCategoria("");
  };

  const actualizarCampo = (index, campo, valor) => {
    const nuevas = [...categorias];
    nuevas[index][campo] = valor;
    setCategorias(nuevas);
  };

  const agregarArchivo = (index, tipo, archivos) => {
    const nuevas = [...categorias];
    const urls = Array.from(archivos).map((file) =>
      URL.createObjectURL(file)
    );
    nuevas[index][tipo] = [...nuevas[index][tipo], ...urls];
    setCategorias(nuevas);
  };

  const enviarReserva = (e) => {
    e.preventDefault();
    alert("Reserva enviada correctamente ✅ (Aquí puedes conectar backend)");
    setReserva({ nombre: "", email: "", fecha: "", servicio: "" });
  };

  return (
    <div className="min-h-screen bg-black text-white font-sans">
      {/* HERO */}
      <section className="text-center py-24 bg-gradient-to-b from-black to-neutral-900">
        <h1 className="text-6xl font-bold mb-4 tracking-wide">Tu Marca</h1>
        <p className="text-neutral-400 text-lg">
          Fotografía Profesional • Calidad Premium • Momentos Eternos
        </p>
      </section>

      {/* ADMINISTRADOR */}
      <section className="p-10 max-w-6xl mx-auto">
        <h2 className="text-3xl font-semibold mb-6">Administrador</h2>

        <div className="flex gap-4 mb-10">
          <input
            type="text"
            placeholder="Nueva carpeta"
            value={nuevaCategoria}
            onChange={(e) => setNuevaCategoria(e.target.value)}
            className="flex-1 p-3 rounded-xl bg-neutral-800"
          />
          <button
            onClick={agregarCategoria}
            className="bg-white text-black px-6 rounded-2xl"
          >
            Crear
          </button>
        </div>

        {categorias.map((cat, index) => (
          <div key={index} className="mb-16 bg-neutral-900 p-6 rounded-2xl">
            <h3 className="text-2xl mb-4">{cat.nombre}</h3>

            <textarea
              placeholder="Descripción"
              value={cat.descripcion}
              onChange={(e) =>
                actualizarCampo(index, "descripcion", e.target.value)
              }
              className="w-full p-3 mb-4 rounded-xl bg-neutral-800"
            />

            <input
              type="text"
              placeholder="Precio"
              value={cat.precio}
              onChange={(e) =>
                actualizarCampo(index, "precio", e.target.value)
              }
              className="w-full p-3 mb-4 rounded-xl bg-neutral-800"
            />

            <div className="mb-6">
              <label>Fotos</label>
              <input
                type="file"
                multiple
                accept="image/*"
                onChange={(e) =>
                  agregarArchivo(index, "fotos", e.target.files)
                }
              />
              <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mt-4">
                {cat.fotos.map((f, i) => (
                  <img
                    key={i}
                    src={f}
                    alt=""
                    className="h-40 w-full object-cover rounded-xl"
                  />
                ))}
              </div>
            </div>

            <div>
              <label>Videos</label>
              <input
                type="file"
                multiple
                accept="video/*"
                onChange={(e) =>
                  agregarArchivo(index, "videos", e.target.files)
                }
              />
              <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                {cat.videos.map((v, i) => (
                  <video key={i} src={v} controls className="rounded-xl" />
                ))}
              </div>
            </div>

            <p className="mt-6 font-bold text-lg">Precio: {cat.precio}</p>
          </div>
        ))}
      </section>

      {/* RESERVAS */}
      <section className="bg-neutral-950 py-20 px-6">
        <h2 className="text-3xl text-center font-semibold mb-10">
          Reservar Sesión
        </h2>

        <form
          onSubmit={enviarReserva}
          className="max-w-2xl mx-auto grid gap-6"
        >
          <input
            type="text"
            placeholder="Nombre"
            value={reserva.nombre}
            onChange={(e) =>
              setReserva({ ...reserva, nombre: e.target.value })
            }
            className="p-3 rounded-xl bg-neutral-800"
          />

          <input
            type="email"
            placeholder="Email"
            value={reserva.email}
            onChange={(e) =>
              setReserva({ ...reserva, email: e.target.value })
            }
            className="p-3 rounded-xl bg-neutral-800"
          />

          <input
            type="date"
            value={reserva.fecha}
            onChange={(e) =>
              setReserva({ ...reserva, fecha: e.target.value })
            }
            className="p-3 rounded-xl bg-neutral-800"
          />

          <select
            value={reserva.servicio}
            onChange={(e) =>
              setReserva({ ...reserva, servicio: e.target.value })
            }
            className="p-3 rounded-xl bg-neutral-800"
          >
            <option value="">Seleccionar servicio</option>
            {categorias.map((cat, i) => (
              <option key={i} value={cat.nombre}>
                {cat.nombre}
              </option>
            ))}
          </select>

          <button className="bg-white text-black py-3 rounded-2xl">
            Confirmar Reserva
          </button>
        </form>
      </section>

      {/* PAGOS */}
      <section className="py-20 text-center bg-neutral-900">
        <h2 className="text-3xl font-semibold mb-6">Pagos Online</h2>
        <p className="text-neutral-400 mb-6">
          Integra Stripe o PayPal aquí para recibir pagos automáticamente.
        </p>
        <button className="bg-white text-black px-8 py-3 rounded-2xl">
          Pagar Ahora
        </button>
      </section>

      {/* FOOTER */}
      <footer className="text-center py-6 text-neutral-500 border-t border-neutral-800">
        © {new Date().getFullYear()} Tu Marca - Portafolio Profesional
      </footer>
    </div>
  );
}
