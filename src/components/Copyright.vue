<template>
    <footer class="text-center">
        <p>
            <small>
                &copy; {{ currentYear }} &bull; 
                <a href="https://toddgoates.com" target="_blank" class="text-secondary">Todd Goates</a>
            </small>
        </p>
    </footer>
</template>

<script>
export default {
  name: "Copyright",
  data() {
    return {
      currentYear: new Date().getFullYear()
    };
  }
};
</script>
