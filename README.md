import { useState } from 'react';
import { Loader2 } from 'lucide-react';
import { AuthProvider, useAuth } from '@/context/AuthContext';
import { AppProvider } from '@/context/AppContext';
import { AuthScreen } from '@/screens/AuthScreen';
import { BottomNav, SettingsButton, type TabId } from '@/components/BottomNav';
import { useApp } from '@/context/AppContext';
import { HomeScreen } from '@/screens/HomeScreen';
import { ChallengesScreen } from '@/screens/ChallengesScreen';
import { TasksScreen } from '@/screens/TasksScreen';
import { NotesScreen } from '@/screens/NotesScreen';
import { SummariesScreen } from '@/screens/SummariesScreen';
import { SettingsScreen } from '@/screens/SettingsScreen';

function MainApp() {
  const [tab, setTab] = useState<TabId>('home');
  const { t } = useApp();

  return (
    <div className="min-h-screen bg-gray-50 dark:bg-gray-950">
      <header className="sticky top-0 z-30 bg-white/80 dark:bg-gray-950/80 backdrop-blur-lg border-b border-gray-200 dark:border-gray-800">
        <div className="max-w-2xl mx-auto flex items-center justify-between px-4 h-14">
          <span className="text-base font-bold text-gray-900 dark:text-white">{t('appName')}</span>
          <SettingsButton onClick={() => setTab('settings')} />
        </div>
      </header>
      <main className="max-w-2xl mx-auto px-4 pt-6 pb-24 min-h-screen">
        {tab === 'home' && <HomeScreen onNavigate={setTab} />}
        {tab === 'challenges' && <ChallengesScreen />}
        {tab === 'tasks' && <TasksScreen />}
        {tab === 'notes' && <NotesScreen />}
        {tab === 'summaries' && <SummariesScreen />}
        {tab === 'settings' && <SettingsScreen />}
      </main>
      <BottomNav active={tab} onChange={setTab} />
    </div>
  );
}

function Gate() {
  const { user, loading } = useAuth();

  if (loading) {
    return (
      <div className="min-h-screen flex items-center justify-center bg-gray-50 dark:bg-gray-950">
        <Loader2 size={32} className="animate-spin text-purple-500" />
      </div>
    );
  }

  if (!user) return <AuthScreen />;
  return <MainApp />;
}

export default function App() {
  return (
    <AppProvider>
      <AuthProvider>
        <Gate />
      </AuthProvider>
    </AppProvider>
  );
}
