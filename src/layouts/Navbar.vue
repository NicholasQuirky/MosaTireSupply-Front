<template>
  <ConfirmDialog />
  <div class="navbar row">
    <Button
      icon="pi pi-bars"
      rounded
      outlined
      class="burger-btn"
      @click="sidebarVisible = true"
    />
    <div class="logo">
      <img
        src="https://res.cloudinary.com/dpm5vdakr/image/upload/v1709128297/mosawebapp/assets/Untitled_design_1_k6o8rv.png"
        alt=""
      />
    </div>
    <div class="nav-links row">
      <RouterLink
        v-for="link in is_staff ? adminLinks : links"
        :key="link.name"
        :to="link.path"
        active-class="active-link"
        >{{ link.name }}</RouterLink
      >
    </div>
    <div class="actions row" style="gap: 10px; align-items: center">
      <p v-if="accessToken" style="margin: 0; color: white">
        Hi! {{ fullName }}
      </p>
      <RouterLink v-if="!accessToken" to="/login"
        ><Button label="Login" class="login-btn"
      /></RouterLink>
      <RouterLink to="/cart"
        ><Button
          v-tooltip.bottom="'Cart'"
          icon="pi pi-shopping-cart"
          rounded
          outlined
          class="cart-btn"
      /></RouterLink>
      <Button
        v-if="accessToken"
        v-tooltip.bottom="'Logout'"
        @click="logout()"
        icon="pi pi-sign-out"
        rounded
        outlined
        class="logout-btn"
      />
    </div>
  </div>
  <Sidebar v-model:visible="sidebarVisible" header="Sidebar" class="sidebar">
    <template #header>
      <div class="logo">
        <img
          src="https://res.cloudinary.com/dpm5vdakr/image/upload/v1709128297/mosawebapp/assets/Untitled_design_1_k6o8rv.png"
          alt=""
        />
      </div>
    </template>
    <div class="sidebar-links col">
      <RouterLink
        v-for="link in is_staff ? adminLinks : links"
        :key="link.name"
        :to="link.path"
        active-class="active-link"
        @click="sidebarVisible = false"
        >{{ link.name }}</RouterLink
      >
      <RouterLink v-if="!accessToken" to="/login"
        ><Button icon="pi pi-sign-in" label="Login" class="login-btn"
      /></RouterLink>

      <Button
        v-if="accessToken"
        icon="pi pi-sign-out"
        label="Logout"
        class="login-btn"
        @click="logout()"
      />
    </div>
  </Sidebar>
</template>
<script>
export default {
  data() {
    return {
      sidebarVisible: false,
      links: [
        { name: "Home", path: "/home" },
        { name: "Our Products", path: "/our-products" },
        { name: "My Orders", path: "/my-orders" },
      ],
      adminLinks: [
        { name: "Accounts", path: "/admin-accounts" },
        { name: "Products", path: "/admin-products" },
        { name: "On-site Orders", path: "/admin-on-site-orders" },
        { name: "Orders Management", path: "/admin-orders" },
        { name: "Activity Logs", path: "/admin-activity-logs" },
      ],
      fullName: null,
      accessToken: null,
      is_staff: null,
    };
  },
  methods: {
    logout() {
      this.$confirm.require({
        message: "Are you sure you want to logout?",
        header: "Logout",
        icon: "pi pi-exclamation-triangle",
        rejectLabel: "No",
        acceptLabel: "Yes",
        accept: () => {
          localStorage.removeItem("token");
          this.accessToken = localStorage.getItem("token");
          localStorage.removeItem("is_staff");
          this.is_staff = null;
          this.$router.push("/");
          window.location.reload();
        },
        reject: () => {},
      });
    },
  },
  mounted() {
    this.accessToken = localStorage.getItem("token");
    this.is_staff =
      localStorage.getItem("is_staff") === "ADMINISTRATOR" ||
      localStorage.getItem("is_staff") === "PRODUCT_MANAGER" ||
      localStorage.getItem("is_staff") === "CONTENT_MANAGER";
    this.fullName = localStorage.getItem("name");
  },
};
</script>
