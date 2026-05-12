
import { useState, useEffect } from 'react';
import { View, Text, ScrollView, TouchableOpacity, TextInput, StyleSheet, StatusBar, ActivityIndicator } from 'react-native';

const SERVER_URL = 'https://pickuppro-server.onrender.com';

const COLORS = {
  bg: '#08090d', card: '#10111a', border: '#1c1d2e',
  accent: '#f97316', green: '#22c55e', red: '#ef4444',
  yellow: '#eab308', blue: '#3b82f6',
  text: '#f0f2f8', muted: '#64748b', surface: '#161720',
};

const CATEGORIES = [
  { id: 'appliances', icon: '🏠', label: 'Appliances', desc: 'Washers, dryers, fridges, stoves', basePrice: 120, color: '#3b82f6', note: '2-person lift required', items: ['Washer / Dryer', 'Refrigerator', 'Stove / Range', 'Dishwasher', 'AC Unit', 'Water Heater'] },
  { id: 'furniture', icon: '🛋️', label: 'Large Furniture', desc: 'Sofas, beds, dressers, tables', basePrice: 95, color: '#a855f7', note: 'Disassembly available', items: ['Sofa / Couch', 'Bed Frame', 'Mattress', 'Dresser', 'Dining Table', 'Wardrobe'] },
  { id: 'motorcycle', icon: '🏍️', label: 'Motorcycles', desc: 'Sport bikes, cruisers, ATVs', basePrice: 150, color: '#f97316', note: 'Trailer equipped drivers only', items: ['Sport Bike', 'Cruiser', 'Dirt Bike', 'Scooter', 'ATV'] },
  { id: 'compressor', icon: '⚙️', label: 'Equipment', desc: 'Compressors, generators, machinery', basePrice: 130, color: '#22c55e', note: 'Weight limit 800 lbs', items: ['Air Compressor', 'Generator', 'Pressure Washer', 'Welding Equipment'] },
  { id: 'boxes', icon: '📦', label: 'Boxes & Small Items', desc: 'Moving boxes, bags, small items', basePrice: 35, color: '#eab308', note: 'Fast pickup available', items: ['Moving Boxes (1-5)', 'Moving Boxes (6-15)', 'Moving Boxes (16+)', 'Bags / Luggage'] },
  { id: 'outdoor', icon: '🌿', label: 'Outdoor & Garden', desc: 'Mowers, grills, hot tubs', basePrice: 100, color: '#10b981', note: 'Hot tubs need 4-person team', items: ['Riding Lawn Mower', 'BBQ Grill', 'Hot Tub', 'Patio Furniture'] },
];

export default function App() {
  const [tab, setTab] = useState('home');
  const [user, setUser] = useState({ name: '', email: '', phone: '', role: 'both' });

  return (
    <View style={{ flex: 1, backgroundColor: COLORS.bg }}>
      <StatusBar barStyle="light-content" backgroundColor={COLORS.bg} />
      <View style={styles.topBar}>
        <Text style={styles.logo}>Pick Up <Text style={{ color: COLORS.accent }}>Pro</Text></Text>
        <Text style={styles.topBarTab}>{{ home: '🏠 Home', request: '📦 Book', driver: '🚗 Drive', profile: '👤 Profile' }[tab]}</Text>
      </View>
      <View style={{ flex: 1 }}>
        {tab === 'home' && <HomeScreen user={user} setTab={setTab} />}
        {tab === 'request' && <RequestScreen user={user} />}
        {tab === 'driver' && <DriverScreen user={user} />}
        {tab === 'profile' && <ProfileScreen user={user} setUser={setUser} />}
      </View>
      <View style={styles.navBar}>
        {[['home','🏠','Home'],['request','📦','Book'],['driver','🚗','Drive'],['profile','👤','Profile']].map(([id, icon, label]) => (
          <TouchableOpacity key={id} style={styles.navItem} onPress={() => setTab(id)}>
            <Text style={{ fontSize: 22, opacity: tab === id ? 1 : 0.35 }}>{icon}</Text>
            <Text style={[styles.navLabel, { color: tab === id ? COLORS.accent : COLORS.muted }]}>{label}</Text>
            {tab === id && <View style={styles.navDot} />}
          </TouchableOpacity>
        ))}
      </View>
    </View>
  );
}

function HomeScreen({ user, setTab }) {
  const [serverOk, setServerOk] = useState(null);
  useEffect(() => {
    fetch(SERVER_URL + '/').then(() => setServerOk(true)).catch(() => setServerOk(false));
  }, []);
  return (
    <ScrollView style={{ flex: 1 }} contentContainerStyle={{ padding: 20, paddingBottom: 100 }}>
      <Text style={styles.greeting}>Welcome back{user.name ? ', ' + user.name.split(' ')[0] : ''} 👋</Text>
      <Text style={styles.title}>Pick Up <Text style={{ color: COLORS.accent }}>Pro</Text></Text>
      <View style={styles.statusRow}>
        <View style={[styles.dot, { backgroundColor: serverOk === null ? COLORS.yellow : serverOk ? COLORS.green : COLORS.red }]} />
        <Text style={styles.statusText}>{serverOk === null ? 'Connecting...' : serverOk ? 'Server online ✓' : 'Server offline'}</Text>
      </View>
      <View style={styles.grid}>
        {[{icon:'📦',label:'Book Pickup',color:COLORS.accent,tab:'request'},{icon:'🚗',label:'Start Driving',color:COLORS.blue,tab:'driver'},{icon:'💰',label:'Earnings',color:COLORS.green,tab:'driver'},{icon:'👤',label:'Profile',color:'#a855f7',tab:'profile'}].map(q => (
          <TouchableOpacity key={q.label} style={styles.quickCard} onPress={() => setTab(q.tab)}>
            <Text style={{ fontSize: 26, marginBottom: 8 }}>{q.icon}</Text>
            <Text style={[styles.quickLabel, { color: q.color }]}>{q.label}</Text>
          </TouchableOpacity>
        ))}
      </View>
      <Text style={styles.sectionTitle}>WHAT WE MOVE</Text>
      <View style={styles.grid}>
        {CATEGORIES.map(cat => (
          <TouchableOpacity key={cat.id} style={styles.catCard} onPress={() => setTab('request')}>
            <Text style={{ fontSize: 22, marginBottom: 6 }}>{cat.icon}</Text>
            <Text style={[styles.catLabel, { color: cat.color }]}>{cat.label}</Text>
            <Text style={styles.catPrice}>from ${cat.basePrice}</Text>
          </TouchableOpacity>
        ))}
      </View>
    </ScrollView>
  );
}

function RequestScreen({ user }) {
  const [step, setStep] = useState('category');
  const [selectedCat, setSelectedCat] = useState(null);
  const [selectedItem, setSelectedItem] = useState(null);
  const [form, setForm] = useState({ pickup: '', dropoff: '', notes: '', price: '', email: user.email || '' });
  const [loading, setLoading] = useState(false);
  const price = parseFloat(form.price) || 0;

  const handleBook = async () => {
    if (!form.pickup || !form.dropoff || !form.email) return alert('Fill in all fields');
    setLoading(true)
