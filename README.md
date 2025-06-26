<!-- Supabase 实时聊天系统 -->
<div id="app">
  <div v-if="!user">
    <h3>登录/注册2.0版本的CKJ-Jayce社交平台！</h3>
    <input v-model="email" placeholder="邮箱" type="email">
    <input v-model="password" placeholder="密码" type="password">
    <button @click="register">注册</button>
    <button @click="login">登录</button>
    <p style="color:red">{{ error }}</p>
  </div>
  <div v-if="user">
    <h3>欢迎, {{ user.email }}！</h3>
    <button @click="logout">退出</button>
    <hr>
    <div v-for="msg in messages" :key="msg.id">
      <strong>{{ msg.user }}:</strong> {{ msg.text }}
    </div>
    <input v-model="newMessage" placeholder="输入消息" @keyup.enter="sendMessage">
    <button @click="sendMessage">发送</button>
  </div>
</div>

<!-- 引入依赖 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.min.js"></script>
<script src="https://unpkg.com/@supabase/supabase-js@2"></script>

<script>
// 初始化 Supabase
const supabase = supabase.createClient(
  "https://goxgchptmcstbzeimtrv.supabase.co",
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdveGdjaHB0bWNzdGJ6ZWltdHJ2Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTA5Mjc0NTksImV4cCI6MjA2NjUwMzQ1OX0.oGcVG8Hc5ph4yQuww-VyuQri-8OcXDN9gSlWgl2fEXk"
);

new Vue({
  el: '#app',
  data: {
    email: '',
    password: '',
    user: null,
    messages: [],
    newMessage: '',
    error: ''
  },
  created() {
    // 检查登录状态
    supabase.auth.getSession().then(({ data }) => {
      if (data.session) {
        this.user = data.session.user;
        this.loadMessages();
        this.setupRealtime();
      }
    });
  },
  methods: {
    async register() {
      const { error } = await supabase.auth.signUp({
        email: this.email,
        password: this.password
      });
      if (error) this.error = error.message;
    },
    async login() {
      const { error } = await supabase.auth.signInWithPassword({
        email: this.email,
        password: this.password
      });
      if (error) this.error = error.message;
    },
    logout() {
      supabase.auth.signOut();
      this.user = null;
    },
    async loadMessages() {
      const { data } = await supabase
        .from('messages')
        .select('*')
        .order('created_at', { ascending: true });
      this.messages = data || [];
    },
    setupRealtime() {
      // 监听新消息
      supabase.channel('public:messages')
        .on(
          'postgres_changes',
          { event: 'INSERT', schema: 'public', table: 'messages' },
          (payload) => {
            this.messages.push(payload.new);
          }
        )
        .subscribe();
    },
    async sendMessage() {
      if (!this.newMessage.trim()) return;
      await supabase
        .from('messages')
        .insert([{ user: this.user.email, text: this.newMessage }]);
      this.newMessage = '';
    }
  }
})
</script>

<style>
#app {
  font-family: Arial;
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #eee;
}
</style>
