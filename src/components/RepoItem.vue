<template>
    <b-container>
        <b-row>
            <b-col sm="auto">
                <img height="50px" :src="repo.owner.avatar_url"/>
            </b-col>
            <b-col>
                <h3>{{repo.name}}</h3>
            </b-col>
            <b-col>
                <p>{{repo.description}}</p>
            </b-col>
            <b-col>{{repo.owner.login}}</b-col>
        </b-row>
    </b-container>
</template>

<script>

export default {
    props: {
        repo: {}
    }
}
</script>

<style scoped>

</style>