
import React from 'react';
import {
  Home,
  Compass,
  Grid,
  Wallet
  Wallet,
  Sparkles
} from 'lucide-react';
import { useAuth } from '../context/AuthContext';
  const navItems = [
    { id: 'home', label: 'Home', icon: Home },
    { id: 'ai-copilot', label: 'AI Copilot', icon: Sparkles },
    { id: 'trip-planner', label: 'Plan Trip', icon: Compass },
    { id: 'services-apps', label: 'Apps', icon: Grid },
    { id: 'budget-planner', label: 'Budget', icon: Wallet }
  );
}
import React, { useState } from 'react';
import Navbar from './components/Navbar';
import HomePage from './components/HomePage';
import EmergencyModal from './components/EmergencyModal';
import MobileBottomNav from './components/MobileBottomNav';
import AuthPage from './components/AuthPage';
import AiTripCopilot from './components/AiTripCopilot';
import { AuthProvider, useAuth } from './context/AuthContext';
import { Compass, ShieldCheck, CheckCircle2, AlertCircle, Info } from 'lucide-react';
function AppContent() {
  const { currentUser, authNotification } = useAuth();
  const [activeTab, setActiveTab] = useState('home'); // 'home' | 'trip-planner' | 'services-apps' | 'budget-planner'
  const [showSOSModal, setShowSOSModal] = useState(false);
  // Shared Trip Planner Parameters
  const [tripParams, setTripParams] = useState({
    origin: 'Hyderabad',
    destination: 'Warangal',
    days: 2,
    members: 2
  });
  // Shared Budget Planner Parameters
  const [budgetParams, setBudgetParams] = useState({
    budget: 5000,
    days: 2,
    origin: 'Hyderabad'
  });
  // Consistent Indian Rupee (₹) Price Formatter
  const formatPrice = (amount) => {
    const num = Number(amount) || 0;
    import React, { useState } from 'react';
import Navbar from './components/Navbar';
import HomePage from './components/HomePage';
import EmergencyModal from './components/EmergencyModal';
import MobileBottomNav from './components/MobileBottomNav';
import AuthPage from './components/AuthPage';
import AiTripCopilot from './components/AiTripCopilot';
import { AuthProvider, useAuth } from './context/AuthContext';
import { Compass, ShieldCheck, CheckCircle2, AlertCircle, Info } from 'lucide-react';
function AppContent() {
  const { currentUser, authNotification } = useAuth();
  const [activeTab, setActiveTab] = useState('home'); // 'home' | 'trip-planner' | 'services-apps' | 'budget-planner'
  const [showSOSModal, setShowSOSModal] = useState(false);
  // Shared Trip Planner Parameters
  const [tripParams, setTripParams] = useState({
    origin: 'Hyderabad',
    destination: 'Warangal',
    days: 2,
     {/* Toast Notification for Auth Events */}
        {authNotification && (
          <div className="fixed top-16 right-4 z-50 max-w-sm p-4 rounded-2xl bg-slate-900/95 border border-emerald-500/40 shadow-2xl backdrop-blur-xl flex items-center gap-3 animate-fadeIn">
            {authNotification.type === 'info' ? (
              <Info className="w-5 h-5 text-cyan-400 flex-shrink-0" />
            ) : authNotification.type === 'error' ? (
              <AlertCircle className="w-5 h-5 text-rose-400 flex-shrink-0" />
            ) : (
              <CheckCircle2 className="w-5 h-5 text-emerald-400 flex-shrink-0" />
            )}
            <span className="text-xs font-semibold text-slate-200">
              {authNotification.message}
            </span>
          </div>
        )}
        <AuthPage onAuthSuccess={() => setActiveTab('home')} />
      </div>
    );
  }
                  <li><span className="text-slate-300">Food: Zomato, Swiggy, IRCTC</span></li>
                <li><span className="text-slate-300">Groceries: Blinkit, Instamart, Zepto</span></li>
                <li><span className="text-slate-300">Medical: Apollo 24|7, Tata 1mg</span></li>
                <li>
                  <button onClick={() => setShowSOSModal(true)} className="text-rose-400 font-bold hover:underline">
                    Emergency Helplines: 112, 108, 1363
                  </button>
                </li>
              </ul>
            </div>
            {/* Safety Guarantee */}
            <div className="space-y-2">
              <span className="text-xs font-bold uppercase tracking-wider text-white block">Device Support</span>
              <p className="text-slate-400 text-xs leading-relaxed">
                Fully responsive layout tailored for smartphones, tablets, and desktops with PWA full-screen support.
              </p>
              <div className="pt-2 flex items-center gap-2 text-emerald-400 font-semibold">
                <ShieldCheck className="w-4 h-4" />
                <span>iOS & Android Ready</span>
              </div>
            </div>
          </div>
          <div className="pt-6 flex flex-col sm:flex-row items-center justify-between gap-3 text-slate-500 text-[11px]">
            <p>© 2026 SmartTrip Planner. All rights reserved.</p>
            <p className="flex items-center gap-1">
              Protected Travel Planning Portal
            </p>
          </div>
        </div>
      </footer>
    </div>
  );
}
export default function App() {
  return (
    <AuthProvider>
      <AppContent />
    </AuthProvider>
  );
}
