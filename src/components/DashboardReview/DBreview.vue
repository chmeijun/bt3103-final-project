<template>
  <div>
    <div class="users">
      <div class="profile-img">
        <img id="profile-pic" v-bind:src="profile_pic" />
      </div>
      <div class="wrapper">
        <div class="date-box">
          <span id="Date"> {{ this.getTime() }}</span>
        </div>
        <span id="Name">{{ this.getName() }}</span>

        <p></p>
        <star-rating
          v-bind:read-only="true"
          v-bind:rating="this.review.rating"
          v-bind:increment="0.1"
          v-bind:show-rating="false"
          v-bind:star-size="16"
          border-color="black"
          v-bind:border-width="3"
          v-bind:rounded-corners="true"
          inactive-color="white"
          active-color="black"
        >
        </star-rating>
        {{ review.review }}
      </div>
    </div>
  </div>
</template>

<script>
import StarRating from "vue-star-rating";
import db from "../../firebase.js";
import moment from "moment";
export default {
  components: {
    "star-rating": StarRating,
  },

  data() {
    return {
      reviewer_name: "",
      rating: 0,
      review_given: "",
      date: "",
      profile_pic: "",
    };
  },

  props: ["review"],

  methods: {
    getName() {
      db.collection("Users")
        .doc(this.review.user_id)
        .get()
        .then((doc) => {
          this.reviewer_name = doc.data().name;
          this.profile_pic = doc.data().image;
        });
      return this.reviewer_name;
    },

    getTime() {
      return moment(this.review.time).format("DD/MM/YYYY");
    },
  },

  created() {},
};
</script>

<style scoped>
#profile-pic {
  border-radius: 50%;
  width: 100px;
  height: 100px;
}

#Name {
  text-decoration: underline;
  color: #565656df;
}

#Date {
  display: flex;
  justify-content: flex-end;
  color:#565656df;
}

.users {
  display: flex;
}

.wrapper {
  width: 80%;
  font-size: 16px;
  padding-right: 20px;
  padding-left: 10px;
  padding-bottom: 15px;
}


.profile-img {
  display: flex;
  justify-content: center; /* align horizontal */
  align-items: center; /* align vertical */
  margin: 20px;
  width: 20%;
}

</style>