// ===============================
// 📁 Projeto React - Candidatura Online (Pronto para GitHub)
// Autor: Daniel
// WhatsApp destino: +55 98 98453-3013
// ===============================
// Arquivo sugerido: src/App.jsx
// Framework sugerido: React + Vite ou Create React App
// TailwindCSS recomendado (opcional)
// ===============================

import React, { useState } from "react";

export default function App() {
  const [formData, setFormData] = useState({
    name: "",
    phone: "",
    experience: "",
    course: "",
    otherCourse: "",
  });
  const [showSuccess, setShowSuccess] = useState(false);
  const [generatedLink, setGeneratedLink] = useState("");

  const courses = [
    "Vendas",
    "Marketing",
    "Informática",
    "Excel",
    "Outros"
  ];

  function handleChange(e) {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  }

  function generateWhatsAppLink() {
    const phoneNumber = "5598984533013";

    let message = `📄 *FICHA DE CANDIDATURA*\n\n`;
    message += `Olá! Segue abaixo minha ficha de inscrição para o processo seletivo:\n\n`;
    message += `👤 *Candidato(a):* ${formData.name}\n`;
    message += `📱 *Contato:* ${formData.phone}\n`;
    message += `💼 *Experiência na área:* ${formData.experience}\n`;
    message += `🎓 *Qualificação/Curso:* ${formData.course}\n`;

    if (formData.course === "Outros") {
      message += `📝 *Detalhes Adicionais:* ${formData.otherCourse}\n`;
    }

    message += `\n-----------------------------------\n`;
    message += `📎 *Obs:* O arquivo do meu Currículo (PDF/Foto) segue em anexo nesta mensagem para análise.\n\n`;
    message += `✅ *Aguardo confirmação de recebimento. Obrigado!*`;

    return `https://wa.me/${phoneNumber}?text=${encodeURIComponent(message)}`;
  }

  function handleSubmit(e) {
    e.preventDefault();

    const url = generateWhatsAppLink();
    setGeneratedLink(url);

    window.open(url, '_blank');

    setShowSuccess(true);
  }

  if (showSuccess) {
    return (
      <div className="min-h-screen bg-gray-50 flex flex-col items-center justify-center p-6 font-sans">
        <div className="max-w-md w-full bg-white rounded-3xl shadow-xl p-8 text-center border border-green-100">

          <div className="w-20 h-20 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-6">
            <svg className="w-10 h-10 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2.5" d="M5 13l4 4L19 7"></path>
            </svg>
          </div>

          <h2 className="text-2xl font-bold text-gray-800 mb-2">Ficha Gerada!</h2>
          <p className="text-gray-600 mb-6">
            Se o WhatsApp não abriu automaticamente, utilize o botão abaixo.
          </p>

          <a 
            href={generatedLink}
            target="_blank"
            rel="noopener noreferrer"
            className="block w-full bg-green-600 hover:bg-green-700 text-white rounded-xl py-4 text-lg font-bold shadow-lg mb-6 flex items-center justify-center gap-2"
          >
            <span>Continuar no WhatsApp</span>
          </a>

          <div className="bg-yellow-50 border border-yellow-200 rounded-xl p-4 mb-6 text-left">
            <p className="text-yellow-800 font-medium text-sm">
              <strong>Atenção:</strong> Para validar sua inscrição, é obrigatório anexar seu currículo na conversa do WhatsApp.
            </p>
          </div>

          <button 
            onClick={() => {
              setShowSuccess(false);
              setFormData({
                name: "",
                phone: "",
                experience: "",
                course: "",
                otherCourse: "",
              });
              setGeneratedLink("");
            }}
            className="text-gray-500 hover:text-gray-700 font-medium text-sm underline"
          >
            Preencher nova ficha
          </button>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 flex flex-col items-center justify-center p-4 font-sans">
      <div className="max-w-2xl w-full bg-white rounded-3xl shadow-xl p-10 border border-gray-100">

        <div className="text-center mb-8">
          <h1 className="text-3xl font-extrabold text-gray-900">Candidatura Online</h1>
          <p className="text-gray-500 mt-2">
            Envie seus dados diretamente ao RH via WhatsApp.
          </p>
        </div>

        <form onSubmit={handleSubmit} className="space-y-6">

          <div>
            <label className="block mb-2 font-semibold text-gray-700">Nome Completo</label>
            <input
              type="text"
              name="name"
              value={formData.name}
              onChange={handleChange}
              className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4"
              required
            />
          </div>

          <div>
            <label className="block mb-2 font-semibold text-gray-700">Telefone / WhatsApp</label>
            <input
              type="tel"
              name="phone"
              value={formData.phone}
              onChange={handleChange}
              className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4"
              required
            />
          </div>

          <div>
            <label className="block mb-2 font-semibold text-gray-700">Possui experiência?</label>
            <div className="flex gap-4">

              <label className={`flex-1 p-4 border-2 rounded-xl text-center cursor-pointer ${formData.experience === 'Sim' ? 'border-blue-500 bg-blue-50' : 'border-gray-200'}`}>
                <input type="radio" name="experience" value="Sim" onChange={handleChange} required /> Sim
              </label>

              <label className={`flex-1 p-4 border-2 rounded-xl text-center cursor-pointer ${formData.experience === 'Não' ? 'border-blue-500 bg-blue-50' : 'border-gray-200'}`}>
                <input type="radio" name="experience" value="Não" onChange={handleChange} /> Não
              </label>
            </div>
          </div>

          <div>
            <label className="block mb-2 font-semibold text-gray-700">Curso / Área</label>
            <select
              name="course"
              value={formData.course}
              onChange={handleChange}
              className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4"
              required
            >
              <option value="">Selecione</option>
              {courses.map(c => (
                <option key={c} value={c}>{c}</option>
              ))}
            </select>
          </div>

          {formData.course === "Outros" && (
            <div>
              <label className="block mb-2 font-semibold text-gray-700">Descreva</label>
              <input
                type="text"
                name="otherCourse"
                value={formData.otherCourse}
                onChange={handleChange}
                className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4"
                required
              />
            </div>
          )}

          <button
            type="submit"
            className="w-full bg-green-600 hover:bg-green-700 text-white rounded-xl py-4 text-lg font-bold"
          >
            Enviar via WhatsApp
          </button>

        </form>
      </div>
    </div>
  );
}
