import React, { useState } from 'react';
import { Mic, Send, Paperclip, LayoutDashboard } from 'lucide-react';

const AISolverPlatform = () => {
  const [input, setInput] = useState("");

  return (
    <div className="min-h-screen bg-[#121212] text-gray-200 font-sans flex flex-col">
      {/* Navigation */}
      <nav className="border-b border-white/5 p-4 flex justify-between items-center bg-[#1a1a1a]">
        <h1 className="text-xl font-bold tracking-tight text-blue-400">AI_SOLVE <span className="text-xs text-gray-500 font-normal">v1.0</span></h1>
        <button className="p-2 hover:bg-white/5 rounded-full transition-colors">
          <LayoutDashboard size={20} />
        </button>
      </nav>

      {/* Main Chat Area */}
      <main className="flex-1 max-w-4xl w-full mx-auto p-6 overflow-y-auto space-y-6">
        {/* Sample AI Response Card */}
        <div className="bg-[#1e1e1e] border border-white/5 rounded-2xl p-6 shadow-xl">
          <p className="text-sm text-blue-400 mb-2 font-medium">AI Solution:</p>
          <p className="leading-relaxed text-gray-300">
            How can I help you today? You can type your problem below or use the microphone for a voice query.
          </p>
        </div>
      </main>

      {/* Matte Input Bar */}
      <div className="p-6 bg-[#121212]">
        <div className="max-w-4xl mx-auto relative">
          <div className="bg-[#252525] border border-white/10 rounded-2xl flex items-center p-2 shadow-2xl transition-all focus-within:border-blue-500/50">
            
            <button className="p-3 text-gray-400 hover:text-white transition-colors" title="Voice Message">
              <Mic size={22} />
            </button>
            
            <input 
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Describe your problem..."
              className="flex-1 bg-transparent border-none outline-none px-2 py-3 text-gray-200 placeholder-gray-500"
            />

            <button className="p-3 text-gray-400 hover:text-white transition-colors">
              <Paperclip size={20} />
            </button>
            
            <button className="bg-blue-600 hover:bg-blue-500 text-white p-3 rounded-xl transition-all ml-2">
              <Send size={20} />
            </button>
          </div>
          <p className="text-center text-[10px] text-gray-600 mt-3 uppercase tracking-widest">
            AI can make mistakes. Verify important info.
          </p>
        </div>
      </div>
    </div>
  );
};

export default AISolverPlatform;
