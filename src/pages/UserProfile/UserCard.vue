<template>
  <card class="card-user">
    <div slot="image">
      <img src="@/assets/img/background.jpg" alt="..." />
    </div>
    <div>
      <div class="author">
        <h4 class="title">
          Administrador
          <br />
        </h4>
        <hr />
        <div class="text-center">
          <div class="row">
            <div
              v-for="(info, index) in details"
              :key="index"
              :class="getClasses(index)"
              class="col-md-4"
            >
              <h5>
                {{ info.title }}
                <br />
                <small>{{ info.subTitle }}</small>
              </h5>
            </div>
          </div>
        </div>
      </div>
    </div>
  </card>
</template>

<script>
export default {
  data() {
    return {
      details: [
        
      ],
    };
  },
  methods: {
    getClasses(index) {
      return {
        "col-md-4": index === 0 || index === 2,
        "col-md-8": index === 1,
      };
    },
  },
};
</script>

<style></style>
