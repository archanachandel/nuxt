 <template>
  <div>
    <button class="btn" @click="creteuser()">Add User</button>

    <v-data-table
      :headers="headers"
      :items="users"
      :items-per-page="10"
      class="elevation-1"
    >
      <template #[`item.avatar`]="{ item }">
        <img :src="item.avatar" />
      </template>
      <template #[`item.actions`]="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
    </v-data-table>
  </div>
</template>
    
  

<script>
export default {
  name: "users",
  data() {
    return {
      headers: [
        { text: "ID", align: "start", sortable: true, value: "id" },
        { text: "Name", value: "name" },
        { text: "Image", value: "avatar" },
        { text: "Email", value: "email" },
        { text: "Created At", value: "createdAt" },
        { text: "Actions", value: "actions", sortable: false },
      ],
      users: [],
    };
  },

  methods: {
    editItem(item) {
      this.$router.push(`/users/${item.id}`);
    },
    deleteItem(item) {
      let c = confirm(
        "Do you really want to delete it? You will not be able to restore this data again!"
      );
      if (c) {
        this.$axios
          .delete(
            `https://62d7f3219c8b5185c77f9c74.mockapi.io/users/${item.id}`
          )
          .then((resp) => {
            this.$router.go();
            // this.$forceUpdate()

            console.warn(resp);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    creteuser() {
      this.$axios
        .post(`https://62d7f3219c8b5185c77f9c74.mockapi.io/users`, {
          name: "archana",
          email: "archana@gmail.com",
          avatar:
            "https://cloudflare-ipfs.com/ipfs/Qmd3W5DuhgHirLHGVixi6V76LhCkZUz6pnFt5AJBiyvHye/avatar/118.jpg",
          createdAt:"2022-07-20T11:27:06.124Z"

        })
        .then((resp) => {
          //this.$toast.success('User updated Successfully').goAway(1000)
          //this.$router.push(`/users`);
          // this.user = resp.user
          // this.user.name = resp.user.name
           console.warn(resp)
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.$axios
      .get("https://62d7f3219c8b5185c77f9c74.mockapi.io/users")
      .then((response) => {
        this.users = response.data;
        // this.users.map((ele) => {
        //   console.log(ele.avatar, "check");
        // });
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>
<style scoped>
.btn {
  border: 2px solid black;
  border-radius: 5px;
  margin: 10px;
  background-color: white;
  color: black;
  padding: 14px 28px;
  font-size: 16px;
  cursor: pointer;
  font-family: "Roboto", sans-serif;
}
</style>
