<script lang="ts">
  import { ref, reactive } from 'vue'

  export default {
    props: {
      cartPrice: Number,
      itemPrice: Number,
    },
    

    data() {
      return {
        cartPrice: 0,
        itemPrice: 0,
        // sectionId: 'template--20592259891485__main', // local
        sectionId: 'template--20577383153949__main',
      }
    },

    methods: {
      getEl(elementSelector: string) {
        return document.querySelector(elementSelector)
      },


      handleQuantityMinusClick(event: any): void {
        // Check that the button is not disabled before updating
        if(!event.target.classList.contains('disabled')) {
          this.cartPrice = this.cartPrice - this.itemPrice
          this.updateColor()
        }
      },

      handleQuantityPlusClick(): void {
        this.cartPrice = this.cartPrice + this.itemPrice
        this.updateColor()
      },

      formatCartPrice(price: number) {
        const intlPrice = new Intl.NumberFormat('en-US', {
          style: 'currency',
          currency: 'USD',
        })
        return intlPrice.format(price)
      },

      updateColor() {
        const customSlot = this.getEl('#Custom-Slot')
        if(customSlot) {
          const cp = this.cartPrice

          // First remove any existing classes from #Custom-Slot
          customSlot.removeAttribute('class')

          if(cp <= 5000) {
            customSlot.classList.add('red')
            return
          }
          if(cp > 10000) {
            customSlot.classList.add('skyblue')
            return
          }
          customSlot.classList.add('green')
          return
        }
      },
    },

    mounted() {
      // Add listener to quantity minus button
      const quantityMinusButton = this.getEl(`#Quantity-Minus-${this.sectionId}`)
      if(quantityMinusButton) {
        quantityMinusButton.addEventListener('click', this.handleQuantityMinusClick)
      }

      // Add listener to quantity plus button
      const quantityPlusButton = this.getEl(`#Quantity-Plus-${this.sectionId}`)
      if(quantityPlusButton) {
        quantityPlusButton.addEventListener('click', this.handleQuantityPlusClick)
      }

      // Set the initial Cart Price and current Item Price
      const prices = this.getEl('#Prices')
      if(prices) {
        this.cartPrice = Number(prices.getAttribute('data-initial-cart-price')?.replace(',', ''))
        this.itemPrice = Number(prices.getAttribute('data-item-price')?.replace(',', ''))
      }

      // Ensure that we have the right starting color
      this.updateColor()
    },

  }
</script>

<template>
  <main>
    <p>Cart Price: {{ formatCartPrice(cartPrice) }}</p>
    <p id="Custom-Slot" class="red">Custom slot</p>
  </main>
</template>

<style scoped>
.green {
  background: green;
}
.red {
  background: red;
}
.skyblue {
  background: skyblue;
}
</style>
