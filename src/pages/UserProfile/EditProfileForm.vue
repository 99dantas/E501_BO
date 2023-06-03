<template>
  <card class="card" title="Perfil">
    <div>
      <form @submit.prevent>
        <div class="row">
          <div class="col-md-3">
            <fg-input
              type="text"
              label="Username"
              placeholder="Username"
              v-model="user.username"
            >
            </fg-input>
          </div>
          <div class="col-md-4">
            <fg-input
              type="email"
              label="Email"
              placeholder="Email"
              v-model="user.email"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-6">
            <fg-input
              type="text"
              label="Nome"
              placeholder="Nome"
              v-model="user.nome"
            >
            </fg-input>
          </div>
        </div>

        <div class="row">
          <div class="col-md-12">
            <div class="form-group">
              <label>About Me</label>
              <textarea
                rows="5"
                class="form-control border-input"
                placeholder="Here can be your description"
                v-model="user.aboutMe"
              >
              </textarea>
            </div>
          </div>
        </div>
        <div class="text-center">
          <p-button type="info" round @click.native.prevent="updateProfile">
            Update Profile
          </p-button>
        </div>
        <div class="clearfix"></div>
      </form>
    </div>
  </card>
</template>

<script>
export default {
  data() {
    return {
      user: {
        username: "",
        email: "",
        nome: "",
        aboutMe: ""
      }
    };
  },
  mounted() {
    this.getAdminProfile();
  },
  methods: {
    getAdminProfile() {
      const userList = JSON.parse(localStorage.getItem("utilizadores"));
      const adminProfile = userList.find(user => user.role === "admin");
      if (adminProfile) {
        this.user = { ...this.user, ...adminProfile };
      }
    },
    updateProfile() {
      alert("Your data: " + JSON.stringify(this.user));
      // Aqui você pode enviar os dados atualizados do perfil do usuário para o servidor
      // para realizar a atualização no banco de dados ou em outras ações necessárias
    }
  }
};
</script>

<style></style>
