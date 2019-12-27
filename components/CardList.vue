<template>
  <section class="card-list">
    <div class="box">
      <div class="title">
        Mis tarjetas
      </div>
      <Card v-for="card in cards" :key="card.number">
        <div class="check-card">
          <img
            alt="Tarjeta predeterminada"
            src="../assets/icons/Ok green ico.svg"
            v-if="card.isDefault"
          />
          <img
            alt="Otras tarjetas"
            src="../assets/icons/Ok grey ico.svg"
            v-else
          />
          <div class="card-image">
            <img alt="Tarjeta" v-bind:src="card.imageSource" />
          </div>
          <div class="card-info">
            <p>{{ card.type }} **** **** **** {{ lastNumbersCard }}</p>
            <p>
              Ex.Date: {{ card.monthExpiration }} /
              {{ card.yearExpiration }}
              <span v-if="card.isDefault">Tarjeta predeterminada</span>
              <button class="btn-card" v-else @click="setDefaultCard(card)">
                Elegir como predeterminada
              </button>
            </p>
          </div>
          <div class="card-btn">
            <button class="remove-btn-card" @click="deleteCard">
              Eliminar
            </button>
          </div>
        </div>
      </Card>
    </div>
    <div class="box">
      <div class="title">
        <button @click="addCard">+ Añadir tarjeta</button>
      </div>
      <div v-if="addCardData">
        <Form :ref="addCardForm" :model="addCardForm" :rules="formValidate">
          <Row type="flex" justify="center">
            <Col span="22">
              <FormItem prop="name" label="Nombre en la tarjeta">
                <Input v-model="addCardForm.name"></Input>
              </FormItem>
            </Col>
            <Col span="22">
              <FormItem
                prop="number"
                label="Número de tarjeta de crédito/débito"
              >
                <Input type="number" v-model="addCardForm.number"></Input>
              </FormItem>
            </Col>
            <Col span="22">
              <FormItem prop="yearExpiration" label="Año de expiración">
                <Input
                  type="number"
                  v-model="addCardForm.yearExpiration"
                ></Input>
              </FormItem>
            </Col>
            <Col span="22">
              <FormItem prop="monthExpiration" label="Mes de expiración">
                <Input
                  type="number"
                  v-model="addCardForm.monthExpiration"
                ></Input>
              </FormItem>
            </Col>
            <Col span="22">
              <FormItem prop="securityCode" label="CVV">
                <Input type="number" v-model="addCardForm.securityCode"></Input>
              </FormItem>
            </Col>
          </Row>
          <FormItem>
            <Row type="flex" justify="space-around">
              <Col :lg="{ span: 4 }">
                <Button @click="cancel">Cancelar</Button></Col
              >
              <Col :lg="{ span: 4 }">
                <Button @click="handleReset">Borrar todo</Button></Col
              >

              <Col :lg="{ span: 4 }">
                <Button type="primary" @click="handleSubmit"
                  >Añadir tarjeta</Button
                ></Col
              >
            </Row>
          </FormItem>
        </Form>
      </div>
    </div>
    <current-modal :show="currentModal" :message="message"></current-modal>
  </section>
</template>
<script>
import localStorage from "localStorage";
import CurrentModal from "@/components/cardActionModal";
export default {
  name: "cardlist",
  components: {
    CurrentModal
  },
  data() {
    return {
      currentModal: false,
      message: {},
      addCardData: false,
      addCardForm: {
        name: "",
        number: "",
        yearExpiration: "",
        monthExpiration: "",
        securityCode: ""
      },
      years: [2020, 2021, 2022, 2023, 2024, 2025, 2026, 2027, 2028, 2029],
      cards: [],
      formValidate: {
        name: [
          {
            required: true,
            message: "Por favor, ingresa el nombre en la tarjeta",
            trigger: "blur"
          }
        ],
        number: [
          {
            required: true,
            message: "Por favor, ingresa el número de tarjeta",
            trigger: "blur"
          },
          {
            min: 16,
            message: "Introduce un número de tarjeta válido",
            trigger: "blur"
          }
        ],
        yearExpiration: [
          {
            required: true,
            message: "Por favor, ingresa el año de expiración",
            trigger: "blur"
          }
        ],
        monthExpiration: [
          {
            required: true,
            message: "Por favor, ingresa el mes de expiración",
            trigger: "blur"
          }
        ],
        securityCode: [
          {
            required: true,
            message: "Por favor, ingresa tu código de seguridad",
            trigger: "blur"
          },
          {
            min: 3,
            message: "Introduce un CVV válido",
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    addCard() {
      this.addCardData = true;
    },
    deleteCard() {
      this.currentModal = true;
      this.message = {
        img: "delete",
        title: "Borrar tarjeta",
        message: "¿Esta seguro de remover su tarjeta?"
      };
    },
    setDefaultCard() {
      this.currentModal = true;
      this.message = {
        img: "default",
        title: "Elegir como tarjeta predeterminada",
        message:
          "Esta tarjeta aparecerá como primera opción. Estas seguro(a) de cambiar esta tarjeta como predeterminada"
      };
    },
    handleSubmit(card) {
      this.$refs[this.addCardForm].validate(valid => {
        if (valid) {
          console.log(this.addCardForm);
          this.cards.push(this.addCardForm);
          localStorage.setItem("cards", JSON.stringify(cards));
          localStorage.setItem("cardNumber", card.number);
          this.$Message.success("Tarjeta añadida con éxito");
        } else {
          this.$Message.error("Revise los campos");
        }
      });
    },
    handleReset() {
      this.$refs[this.addCardForm].resetFields();
    },
    cancel() {
      this.addCardData = false;
    }
  },
  computed: {
    lastNumbersCard: function() {
      return this.addCardForm.number.slice(12);
    }
  }
};
</script>
