<template>
  <section class="card-list">
    <div class="box">
      <Row type="flex" justify="space-between">
        <Col span="4">
          <div class="title">
            Mis tarjetas
          </div>
        </Col>
        <Col span="4">
          <button class="add" @click="addCard">+ Añadir tarjeta</button>
        </Col>
      </Row>

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
                prop="cardNumber"
                label="Número de tarjeta de crédito/débito"
              >
                <Input type="number" v-model="addCardForm.cardNumber"></Input>
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
      <div v-else>
        <Card v-for="card in cards" :key="card.cardNumber">
          <div v-if="card.isDefault">
            <div class="card-default">
              <p>Default</p>
              <p>
                {{ card.type }} **** **** **** {{ card.cardNumber.slice(12) }}
              </p>
              <p>
                Ex.Date: {{ card.monthExpiration }} /
                {{ card.yearExpiration }}
                <!-- <span v-if="card.isDefault">Tarjeta predeterminada</span>
              <button class="btn-card" v-else @click="setDefaultCard(card)">
                Elegir como predeterminada
              </button> -->
              </p>
            </div>
          </div>
          <div v-else>
            <Row type="flex" justify="space-around">
              <Col span="17">
                <div class="card">
                  <p>
                    {{ card.type }} **** **** ****
                    {{ card.cardNumber.slice(12) }}
                  </p>
                  <p>
                    Ex.Date: {{ card.monthExpiration }} /
                    {{ card.yearExpiration }}
                    <!-- <span v-if="card.isDefault">Tarjeta predeterminada</span>
              <button class="btn-card" v-else @click="setDefaultCard(card)">
                Elegir como predeterminada
              </button> -->
                  </p>
                </div></Col
              >
              <Col span="3">
                <button
                  class="remove-btn-card"
                  @click="deleteCard(card.cardNumber)"
                >
                  Eliminar
                </button></Col
              >
              <Col span="3">
                <button
                  class="default-btn-card"
                  @click="setDefaultCard(card.cardNumber)"
                >
                  Predeterminada
                </button>
              </Col>
            </Row>

            <!-- <div class="card-btn"></div> -->
          </div>
        </Card>
      </div>
    </div>
    <current-modal
      :show="currentModal"
      :message="message"
      :cardId="cardId"
    ></current-modal>
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
      cardId: "",
      cards: [],
      addCardData: false,
      addCardForm: {
        name: "Andrea",
        cardNumber: "1234567890123456",
        yearExpiration: "2022",
        monthExpiration: "12",
        securityCode: "123"
      },
      years: [2020, 2021, 2022, 2023, 2024, 2025, 2026, 2027, 2028, 2029],
      formValidate: {
        name: [
          {
            required: true,
            message: "Por favor, ingresa el nombre en la tarjeta",
            trigger: "blur"
          }
        ],
        cardNumber: [
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
    deleteCard(card) {
      this.cardId = card;
      this.currentModal = true;
      this.message = {
        img: "delete",
        title: "Borrar tarjeta",
        message: "¿Esta seguro de remover su tarjeta?"
      };
    },
    setDefaultCard(card) {
      this.cardId = card;
      this.currentModal = true;
      this.message = {
        img: "default",
        title: "Elegir como tarjeta predeterminada",
        message:
          "Esta tarjeta aparecerá como primera opción. Estas seguro(a) de cambiar esta tarjeta como predeterminada"
      };
    },
    handleSubmit() {
      this.$refs[this.addCardForm].validate(valid => {
        if (valid) {
          debugger;
          const payload = { ...this.addCardForm };
          console.log(payload);
          if (this.cards.length < 1) {
            payload.isDefault = true;
          } else {
            payload.isDefault = false;
          }
          this.cards.push(payload);
          localStorage.setItem("cards", JSON.stringify(this.cards));
          this.addCardData = false;
          this.addCardForm = {};
          console.log(this.cards.length);
          this.cards = JSON.parse(localStorage.getItem("cards"));
          this.$Message.success("Tarjeta añadida con éxito");
          console.log(this.cards);
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
  mounted() {
    this.cards = JSON.parse(localStorage.getItem("cards"));
  }
};
</script>
