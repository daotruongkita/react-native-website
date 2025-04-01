import React, { useState, useEffect } from 'react'; import { View, Text, Button } from 'react-native'; import { WebView } from 'react-native-webview';

const App = () => { const [showWebView, setShowWebView] = useState(false); const [webUrl, setWebUrl] = useState(''); const [userData, setUserData] = useState(null);

const handleRegister = () => { setWebUrl('https://yourwebsite.com/register'); setShowWebView(true); };

const handleLogin = () => { setWebUrl('https://yourwebsite.com/login'); setShowWebView(true); };

const handleNavigationStateChange = (event) => { if (event.url.includes('success')) { // Giả lập dữ liệu sau khi đăng nhập thành công setUserData({ fullname: 'Nguyễn Văn A', balance: '10.000 VNĐ' }); setShowWebView(false); } };

return ( <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}> {showWebView ? ( <WebView source={{ uri: webUrl }} onNavigationStateChange={handleNavigationStateChange} style={{ width: '100%', height: '100%' }} /> ) : userData ? ( <View> <Text>Họ và tên: {userData.fullname}</Text> <Text>Số dư: {userData.balance}</Text> </View> ) : ( <View> <Button title="Đăng ký" onPress={handleRegister} /> <Button title="Đăng nhập" onPress={handleLogin} /> </View> )} </View> ); };

export default App;
