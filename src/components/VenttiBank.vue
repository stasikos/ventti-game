<template>
<div>
    Bank: {{ bank.balance }}
</div>
</template>

<script>

export default {
    name: 'VenttiBank',
    props: {
        bank: {
            type: Object,
            required: true
        }
    },
}
</script>