<script setup>
import { onMounted, ref } from 'vue'
import {ethers, utils} from 'ethers'
import abi from '../../contracts/abi.json'
defineProps({
  msg: String
})
// ประกาศให้เป็นglobal
let contract = {}





const count = ref(0)
const amount = ref("0")
const contractName = ref("")
const totalBalance = ref("")
const myAccount = ref("")
const balance = ref("")
const myBalance = ref("")



onMounted(async()=>{
  // console.log(window.ethereum);
  const provider = new ethers.providers.Web3Provider(window.ethereum)
  // Sign Transaction
  const account = await provider.getSigner().getAddress()
  contract = new ethers.Contract(
    '0x4289887C8d882c9eC181bB43c9f516DD7411FeC1',
    abi,
    provider.getSigner()
  )

  contractName.value = await contract.name()
  console.log(contractName)

  // const totalBalance = await contract.totalBalance()
  myAccount.value = account
  totalBalance.value = utils.formatEther(await contract.totalBalance())
  balance.value = utils.formatEther(await contract.balance())
  myBalance.value = utils.formatEther(await contract.balances(account))
  // console.log(utils.formatEther(totalBalance))

})


const deposit = async()=>{
  // console.log(amount.value)
  const tx = await contract.deposit({value: utils.parseEther(amount.value)})
  console.log(tx.hash)
  await tx.wait()
  window.location.reload()
}



const withdraw = async()=>{
  // console.log(amount.value)
  const tx = await contract.withdraw(utils.parseEther(amount.value))
  console.log(tx.hash)
  await tx.wait()
  window.location.reload()
}

</script>

<template>
  <h1>{{ msg }}</h1>
  <h1>{{ amount }}</h1>
  <h1>{{ contractName }}</h1>
  <h1>WALLET: {{ myAccount }}</h1>
  <h1>{{ totalBalance }}</h1>
  <h1>{{ balance }}</h1>
  <h1>MY BALANCES: {{ myBalance }}</h1>
  <p>
    Recommended IDE setup:
    <a href="https://code.visualstudio.com/" target="_blank">VSCode</a>
    +
    <a href="https://github.com/johnsoncodehk/volar" target="_blank">Volar</a>
  </p>

  <p>
    <a href="https://vitejs.dev/guide/features.html" target="_blank">
      Vite Documentation
    </a>
    |
    <a href="https://v3.vuejs.org/" target="_blank">Vue 3 Documentation</a>
  </p>

  <button type="button" @click="count++">count is: {{ count }}</button>
  <input v-model="amount" />
  <button type="button" @click="deposit">Deposit</button>
  <button type="button" @click="withdraw">Withdraw</button>
  <p>
    Edit
    <code>components/HelloWorld.vue</code> to test hot module replacement.
  </p>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
