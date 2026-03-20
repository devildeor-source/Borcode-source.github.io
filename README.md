import React, { useState, useEffect, useRef } from 'react';
import { Mic, Send, Paperclip, StopCircle, User, Bot, Settings } from 'lucide-react';

const AISolverFrontend = () => {
  const [input, setInput] = useState("");
  const [isListening, setIsListening] = useState(false);
  const [messages, setMessages] = useState([
    { role: 'ai', content: 'Hello. I am ready to solve your problems. You can type below or click the mic to speak.' }
  ]);
  
  // Voice Recognition Logic
  const recognitionRef = useRef(null);

  useEffect(() => {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (SpeechRecognition) {
      recognitionRef.current = new SpeechRecognition();
      recognitionRef.current.continuous = false;
      recognitionRef.current.interimResults = false;

      recognitionRef.current.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        setInput(transcript);
        setIsListening(false);
      };

      recognitionRef.current.onerror = () => setIsListening(false);
    }
  }, []);

  const toggleVoice = () => {
    if (isListening) {
      recognitionRef.current.stop();
      setIsListening(false);
    } else {
      setIsListening(true);
      recognitionRef.current.start();
    }
  };

  const handleSend = () => {
    if (!input.trim()) return;
    setMessages([...messages, { role: 'user', content: input }]);
    setInput("");
    // Backend integration will go here later
  };

  return (
    <div className="min-h-screen bg-[#0F0F0F] text-[#E0E0E0] font-sans flex flex-col items-center">
      
      {/* 1. Header Navigation */}
      <nav className="w-full max-w-5xl flex justify-between items-center p-6 bg-[#0F0F0F]">
        <div className="flex items-center gap-2">
          <div className="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center font-bold text-white shadow-lg shadow-blue-900/20">A</div>
          <span className="text-lg font-semibold tracking-tight">AI<span className="text-blue-500 underline decoration-2 underline-offset-4">SOLVE</span></span>
        </div>
        <div className="flex gap-4 opacity-70 hover:opacity-100 transition-opacity cursor-pointer">
          <Settings size={20} />
          <User size={20} />
        </div>
      </nav>

      {/* 2. Chat Display Area */}
      <section className="flex-1 w-full max-w-3xl overflow-y-auto px-4 py-8 space-y-8 scrollbar-hide">
        {messages.map((msg, i) => (
          <div key={i} className={`flex gap-4 ${msg.role === 'user' ? 'justify-end' : 'justify-start'}`}>
            {msg.role === 'ai' && <div className="w-8 h-8 rounded-full bg-[#1E1E1E] border border-white/10 flex items-center justify-center mt-1"><Bot size={16}/></div>}
            
            <div className={`max-w-[80%] p-4 rounded-2xl text-sm leading-relaxed ${
              msg.role === 'user' 
              ? 'bg-blue-600 text-white rounded-tr-none shadow-lg shadow-blue-900/20' 
              : 'bg-[#1A1A1A] border border-white/5 text-gray-300 rounded-tl-none'
            }`}>
              {msg.content}
            </div>
          </div>
        ))}
      </section>

      {/* 3. The Matte Input Component */}
      <footer className="w-full max-w-3xl p-6 pb-10">
        <div className="relative group">
          {/* Outer Matte Container */}
          <div className="bg-[#181818] border border-white/10 rounded-[24px] p-2 flex items-center transition-all duration-300 focus-within:border-blue-500/40 focus-within:bg-[#1C1C1C] shadow-2xl">
            
            {/* Voice Button */}
            <button 
              onClick={toggleVoice}
              className={`p-4 rounded-xl transition-all ${isListening ? 'bg-red-500/20 text-red-500 animate-pulse' : 'hover:bg-white/5 text-gray-400 hover:text-white'}`}
            >
              {isListening ? <StopCircle size={22} /> : <Mic size={22} />}
            </button>

            {/* Input Field */}
            <input 
              type="text" 
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => e.key === 'Enter' && handleSend()}
              placeholder="How can I solve your problem today?"
              className="flex-1 bg-transparent border-none outline-none px-4 py-2 text-[15px] placeholder-gray-600"
            />

            {/* Action Buttons */}
            <div className="flex items-center gap-1 pr-2">
              <button className="p-3 text-gray-500 hover:text-white transition-colors">
                <Paperclip size={20} />
              </button>
              <button 
                onClick={handleSend}
                className="bg-blue-600 hover:bg-blue-500 p-3 rounded-xl text-white transition-all transform active:scale-95"
              >
                <Send size={20} />
              </button>
            </div>
          </div>

          {/* Status Label */}
          <div className="absolute -bottom-6 left-1/2 -translate-x-1/2 flex items-center gap-2 opacity-40">
            <span className="text-[10px] uppercase tracking-[2px]">Encrypted Matte Connection</span>
          </div>
        </div>
      </footer>
    </div>
  );
};

export default AISolverFrontend;
