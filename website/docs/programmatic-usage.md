export default function SistemaComandas() {
  const { useState } = React;

  const [platos, setPlatos] = useState([
    { id: 1, nombre: 'Lomo Saltado', precio: 25 },
    { id: 2, nombre: 'Arroz Chaufa', precio: 18 },
  ]);

  const [bebidas, setBebidas] = useState([
    { id: 1, nombre: 'Inca Kola', precio: 5 },
    { id: 2, nombre: 'Coca Cola', precio: 5 },
  ]);

  const [extras, setExtras] = useState([
    { id: 1, nombre: 'Vaso roto', precio: 3 },
    { id: 2, nombre: 'Plato roto', precio: 5 },
  ]);

  const [comandas, setComandas] = useState([]);
  const [mesa, setMesa] = useState('');
  const [mozo, setMozo] = useState('');
  const [detalle, setDetalle] = useState([]);

  const agregarDetalle = (tipo, item) => {
    setDetalle([
      ...detalle,
      {
        tipo,
        nombre: item.nombre,
        precio: item.precio,
        cantidad: 1,
      },
    ]);
  };

  const cambiarCantidad = (index, cantidad) => {
    const nuevos = [...detalle];
    nuevos[index].cantidad = cantidad;
    setDetalle(nuevos);
  };

  const eliminarDetalle = (index) => {
    const nuevos = detalle.filter((_, i) => i !== index);
    setDetalle(nuevos);
  };

  const total = detalle.reduce(
    (acc, item) => acc + item.precio * item.cantidad,
    0
  );

  const guardarComanda = () => {
    if (!mesa || !mozo || detalle.length === 0) {
      alert('Completa todos los datos');
      return;
    }

    const nueva = {
      id: Date.now(),
      mesa,
      mozo,
      detalle,
      total,
    };

    setComandas([...comandas, nueva]);
    setMesa('');
    setMozo('');
    setDetalle([]);
  };

  const eliminarComanda = (id) => {
    setComandas(comandas.filter((c) => c.id !== id));
  };

  const editarItem = (lista, setLista, index, campo, valor) => {
    const nuevaLista = [...lista];
    nuevaLista[index][campo] = campo === 'precio' ? Number(valor) : valor;
    setLista(nuevaLista);
  };

  const agregarNuevo = (lista, setLista) => {
    setLista([
      ...lista,
      {
        id: Date.now(),
        nombre: 'Nuevo',
        precio: 0,
      },
    ]);
  };

  return (
    <div className="min-h-screen bg-gray-100 p-6">
      <h1 className="text-4xl font-bold mb-6 text-center">
        Sistema de Comandas para Restaurante
      </h1>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div className="bg-white rounded-2xl shadow p-4">
          <h2 className="text-2xl font-semibold mb-4">Platos</h2>

          {platos.map((p, index) => (
            <div key={p.id} className="flex gap-2 mb-2">
              <input
                className="border p-2 rounded w-full"
                value={p.nombre}
                onChange={(e) =>
                  editarItem(platos, setPlatos, index, 'nombre', e.target.value)
                }
              />
              <input
                type="number"
                className="border p-2 rounded w-24"
                value={p.precio}
                onChange={(e) =>
                  editarItem(platos, setPlatos, index, 'precio', e.target.value)
                }
              />
              <button
                onClick={() => agregarDetalle('Plato', p)}
                className="bg-blue-500 text-white px-3 rounded"
              >
                +
              </button>
            </div>
          ))}

          <button
            onClick={() => agregarNuevo(platos, setPlatos)}
            className="bg-green-600 text-white px-4 py-2 rounded mt-2"
          >
            Agregar Plato
          </button>
        </div>

        <div className="bg-white rounded-2xl shadow p-4">
          <h2 className="text-2xl font-semibold mb-4">Bebidas</h2>

          {bebidas.map((b, index) => (
            <div key={b.id} className="flex gap-2 mb-2">
              <input
                className="border p-2 rounded w-full"
                value={b.nombre}
                onChange={(e) =>
                  editarItem(bebidas, setBebidas, index, 'nombre', e.target.value)
                }
              />
              <input
                type="number"
                className="border p-2 rounded w-24"
                value={b.precio}
                onChange={(e) =>
                  editarItem(bebidas, setBebidas, index, 'precio', e.target.value)
                }
              />
              <button
                onClick={() => agregarDetalle('Bebida', b)}
                className="bg-blue-500 text-white px-3 rounded"
              >
                +
              </button>
            </div>
          ))}

          <button
            onClick={() => agregarNuevo(bebidas, setBebidas)}
            className="bg-green-600 text-white px-4 py-2 rounded mt-2"
          >
            Agregar Bebida
          </button>
        </div>

        <div className="bg-white rounded-2xl shadow p-4">
          <h2 className="text-2xl font-semibold mb-4">Extras</h2>

          {extras.map((e, index) => (
            <div key={e.id} className="flex gap-2 mb-2">
              <input
                className="border p-2 rounded w-full"
                value={e.nombre}
                onChange={(ev) =>
                  editarItem(extras, setExtras, index, 'nombre', ev.target.value)
                }
              />
              <input
                type="number"
                className="border p-2 rounded w-24"
                value={e.precio}
                onChange={(ev) =>
                  editarItem(extras, setExtras, index, 'precio', ev.target.value)
                }
              />
              <button
                onClick={() => agregarDetalle('Extra', e)}
                className="bg-blue-500 text-white px-3 rounded"
              >
                +
              </button>
            </div>
          ))}

          <button
            onClick={() => agregarNuevo(extras, setExtras)}
            className="bg-green-600 text-white px-4 py-2 rounded mt-2"
          >
            Agregar Extra
          </button>
        </div>
      </div>

      <div className="bg-white rounded-2xl shadow p-6 mt-6">
        <h2 className="text-3xl font-bold mb-4">Nueva Comanda</h2>

        <div className="grid md:grid-cols-2 gap-4 mb-4">
          <input
            type="text"
            placeholder="Mesa"
            className="border p-3 rounded"
            value={mesa}
            onChange={(e) => setMesa(e.target.value)}
          />

          <input
            type="text"
            placeholder="Mozo que atendió"
            className="border p-3 rounded"
            value={mozo}
            onChange={(e) => setMozo(e.target.value)}
          />
        </div>

        <table className="w-full border mb-4">
          <thead>
            <tr className="bg-gray-200">
              <th className="border p-2">Tipo</th>
              <th className="border p-2">Nombre</th>
              <th className="border p-2">Precio</th>
              <th className="border p-2">Cantidad</th>
              <th className="border p-2">Subtotal</th>
              <th className="border p-2">Acción</th>
            </tr>
          </thead>

          <tbody>
            {detalle.map((d, index) => (
              <tr key={index}>
                <td className="border p-2">{d.tipo}</td>
                <td className="border p-2">{d.nombre}</td>
                <td className="border p-2">S/ {d.precio}</td>
                <td className="border p-2">
                  <input
                    type="number"
                    min="1"
                    value={d.cantidad}
                    onChange={(e) =>
                      cambiarCantidad(index, Number(e.target.value))
                    }
                    className="border p-1 w-20 rounded"
                  />
                </td>
                <td className="border p-2">
                  S/ {(d.precio * d.cantidad).toFixed(2)}
                </td>
                <td className="border p-2">
                  <button
                    onClick={() => eliminarDetalle(index)}
                    className="bg-red-500 text-white px-3 py-1 rounded"
                  >
                    Eliminar
                  </button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>

        <div className="flex justify-between items-center">
          <h3 className="text-2xl font-bold">Total: S/ {total.toFixed(2)}</h3>

          <button
            onClick={guardarComanda}
            className="bg-green-700 text-white px-6 py-3 rounded-xl"
          >
            Guardar Comanda
          </button>
        </div>
      </div>

      <div className="bg-white rounded-2xl shadow p-6 mt-6">
        <h2 className="text-3xl font-bold mb-4">Comandas Registradas</h2>

        {comandas.map((c) => (
          <div key={c.id} className="border rounded-xl p-4 mb-4">
            <div className="flex justify-between mb-2">
              <div>
                <p><strong>Mesa:</strong> {c.mesa}</p>
                <p><strong>Mozo:</strong> {c.mozo}</p>
              </div>

              <div>
                <p className="text-xl font-bold">S/ {c.total.toFixed(2)}</p>
              </div>
            </div>

            <ul className="list-disc ml-6 mb-3">
              {c.detalle.map((d, i) => (
                <li key={i}>
                  {d.nombre} - {d.cantidad} x S/ {d.precio}
                </li>
              ))}
            </ul>

            <button
              onClick={() => eliminarComanda(c.id)}
              className="bg-red-600 text-white px-4 py-2 rounded"
            >
              Eliminar Comanda
            </button>
          </div>
        ))}
      </div>
    </div>
  );
}
