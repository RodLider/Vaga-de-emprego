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
    
    // Formatação Profissional da Mensagem
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
    
    // Tenta abrir diretamente
    window.open(url, '_blank');
    
    // Mostra a tela de sucesso
    setShowSuccess(true);
  }

  // Tela de Sucesso / Próximos Passos
  if (showSuccess) {
    return (
      <div className="min-h-screen bg-gray-50 flex flex-col items-center justify-center p-6 font-sans">
        <div className="max-w-md w-full bg-white rounded-3xl shadow-xl p-8 text-center animate-fade-in-up border border-green-100">
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
            className="block w-full bg-green-600 hover:bg-green-700 text-white rounded-xl py-4 text-lg font-bold shadow-lg mb-6 flex items-center justify-center gap-2 transform active:scale-95 transition-all"
          >
             <span>Continuar no WhatsApp</span>
             <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" fill="currentColor" viewBox="0 0 256 256"><path d="M187.58,144.84l-32-16a8,8,0,0,0-8,.5l-14.69,9.8a40.55,40.55,0,0,1-16-16l9.8-14.69a8,8,0,0,0,.5-8l-16-32A8,8,0,0,0,104,64l-40,13.33a8,8,0,0,0-5.33,7.59c-.06,21.6,7,44.25,20.45,64.71a120.44,120.44,0,0,0,51.27,45c19.14,9.6,39.69,14.68,59.87,14.68h1.26a8,8,0,0,0,7.59-5.33l13.33-40A8,8,0,0,0,187.58,144.84ZM192,201.29A103.56,103.56,0,0,1,146.47,162a103.56,103.56,0,0,1-39.29-45.53L120.61,112,128,123.1a8,8,0,0,0,11.23,2.23,24.4,24.4,0,0,1,33.58,0,8,8,0,0,0,11.23-2.23L195.12,112ZM232,128A104,104,0,1,1,128,24,104.11,104.11,0,0,1,232,128Zm-16,0a88,88,0,1,0-88,88A88.1,88.1,0,0,0,216,128Z"></path></svg>
          </a>
          
          <div className="bg-yellow-50 border border-yellow-200 rounded-xl p-4 mb-6 text-left">
            <div className="flex gap-3">
              <div className="flex-shrink-0 mt-1">
                 <svg className="w-5 h-5 text-yellow-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path></svg>
              </div>
              <p className="text-yellow-800 font-medium text-sm leading-relaxed">
                <strong>Atenção:</strong> Para validar sua inscrição, é <strong>obrigatório</strong> anexar o arquivo do seu currículo na conversa do WhatsApp que será aberta.
              </p>
            </div>
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

  // Tela do Formulário Principal
  return (
    <div className="min-h-screen bg-gray-50 flex flex-col items-center justify-center p-4 sm:p-6 font-sans">
      <div className="max-w-2xl w-full bg-white rounded-3xl shadow-xl p-8 sm:p-10 border border-gray-100">
        <div className="text-center mb-8">
          <div className="inline-block p-3 bg-blue-50 rounded-2xl mb-4">
             {/* Ícone de Documento/Job */}
             <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="#2563eb" viewBox="0 0 256 256"><path d="M216,40H136V24a8,8,0,0,0-8-8H64a8,8,0,0,0-8,8V40H40A16,16,0,0,0,24,56V176a16,16,0,0,0,16,16H216a16,16,0,0,0,16-16V56A16,16,0,0,0,216,40ZM72,32h48V40H72ZM216,176H40V56H216V176ZM232,208a8,8,0,0,1-8,8H32a8,8,0,0,1,0-16H224A8,8,0,0,1,232,208Z"></path></svg>
          </div>
          <h1 className="text-2xl sm:text-3xl font-extrabold text-gray-900 tracking-tight">Candidatura Online</h1>
          <p className="text-gray-500 mt-2 text-base sm:text-lg">
            Preencha seus dados para envio imediato ao setor de RH via WhatsApp.
          </p>
        </div>

        <form onSubmit={handleSubmit} className="space-y-6">
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div className="col-span-1 md:col-span-2">
              <label className="block mb-2 font-semibold text-gray-700 text-sm uppercase tracking-wide">Nome Completo</label>
              <input
                type="text"
                name="name"
                value={formData.name}
                onChange={handleChange}
                className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4 text-gray-900 focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all"
                placeholder="Ex: João da Silva"
                required
              />
            </div>

            <div className="col-span-1 md:col-span-2">
              <label className="block mb-2 font-semibold text-gray-700 text-sm uppercase tracking-wide">Telefone / WhatsApp</label>
              <input
                type="tel"
                name="phone"
                value={formData.phone}
                onChange={handleChange}
                className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4 text-gray-900 focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all"
                placeholder="(00) 90000-0000"
                required
              />
            </div>
          </div>

          <div>
            <label className="block mb-3 font-semibold text-gray-700 text-sm uppercase tracking-wide">Possui experiência na área?</label>
            <div className="flex gap-4">
              <label className={`flex-1 flex items-center justify-center gap-3 p-4 rounded-xl border-2 cursor-pointer transition-all ${formData.experience === 'Sim' ? 'border-blue-500 bg-blue-50 text-blue-700' : 'border-gray-200 hover:border-gray-300'}`}>
                <input
                  type="radio"
                  name="experience"
                  value="Sim"
                  onChange={handleChange}
                  checked={formData.experience === 'Sim'}
                  className="w-5 h-5 text-blue-600 focus:ring-blue-500"
                  required
                />
                <span className="font-medium">Sim, possuo</span>
              </label>

              <label className={`flex-1 flex items-center justify-center gap-3 p-4 rounded-xl border-2 cursor-pointer transition-all ${formData.experience === 'Não' ? 'border-blue-500 bg-blue-50 text-blue-700' : 'border-gray-200 hover:border-gray-300'}`}>
                <input
                  type="radio"
                  name="experience"
                  value="Não"
                  onChange={handleChange}
                  checked={formData.experience === 'Não'}
                  className="w-5 h-5 text-blue-600 focus:ring-blue-500"
                />
                <span className="font-medium">Não possuo</span>
              </label>
            </div>
          </div>

          <div>
            <label className="block mb-2 font-semibold text-gray-700 text-sm uppercase tracking-wide">Qualificação / Curso</label>
            <div className="relative">
              <select
                name="course"
                value={formData.course}
                onChange={handleChange}
                className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4 text-gray-900 appearance-none focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all"
                required
              >
                <option value="">Selecione a área de interesse</option>
                {courses.map(c => (
                  <option key={c} value={c}>{c}</option>
                ))}
              </select>
              <div className="pointer-events-none absolute inset-y-0 right-0 flex items-center px-4 text-gray-500">
                <svg className="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                  <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/>
                </svg>
              </div>
            </div>
          </div>

          {formData.course === "Outros" && (
            <div className="animate-fade-in-up">
              <label className="block mb-2 font-semibold text-gray-700 text-sm uppercase tracking-wide">Descreva sua qualificação</label>
              <input
                type="text"
                name="otherCourse"
                value={formData.otherCourse}
                onChange={handleChange}
                className="w-full bg-gray-50 border border-gray-200 rounded-xl p-4 text-gray-900 focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all"
                placeholder="Ex: Auxiliar Administrativo"
                required
              />
            </div>
          )}

          <div className="pt-4">
            <button
              type="submit"
              className="w-full bg-green-600 hover:bg-green-700 text-white rounded-xl py-4 text-lg font-bold shadow-lg hover:shadow-xl transform active:scale-[0.98] transition-all flex items-center justify-center gap-2"
            >
              <span>Enviar Inscrição (WhatsApp)</span>
              <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="currentColor" viewBox="0 0 256 256"><path d="M187.58,144.84l-32-16a8,8,0,0,0-8,.5l-14.69,9.8a40.55,40.55,0,0,1-16-16l9.8-14.69a8,8,0,0,0,.5-8l-16-32A8,8,0,0,0,104,64l-40,13.33a8,8,0,0,0-5.33,7.59c-.06,21.6,7,44.25,20.45,64.71a120.44,120.44,0,0,0,51.27,45c19.14,9.6,39.69,14.68,59.87,14.68h1.26a8,8,0,0,0,7.59-5.33l13.33-40A8,8,0,0,0,187.58,144.84ZM192,201.29A103.56,103.56,0,0,1,146.47,162a103.56,103.56,0,0,1-39.29-45.53L120.61,112,128,123.1a8,8,0,0,0,11.23,2.23,24.4,24.4,0,0,1,33.58,0,8,8,0,0,0,11.23-2.23L195.12,112ZM232,128A104,104,0,1,1,128,24,104.11,104.11,0,0,1,232,128Zm-16,0a88,88,0,1,0-88,88A88.1,88.1,0,0,0,216,128Z"></path></svg>
            </button>
            
            <div className="mt-4 bg-gray-50 border border-gray-200 p-4 rounded-xl flex gap-3 items-center">
               <div className="flex-shrink-0">
                  <svg className="w-5 h-5 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
               </div>
               <p className="text-sm text-gray-600">
                 Ao clicar, você será redirecionado para enviar a mensagem formatada. Não esqueça de anexar seu <strong>PDF ou Imagem do Currículo</strong>.
               </p>
            </div>
          </div>
        </form>
      </div>
    </div>
  );
}


