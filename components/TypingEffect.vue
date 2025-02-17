<template>
    <span>{{ displayedText || '|' }}</span>
</template>

<script>
export default {
    name: 'TypingEffect',
    props: {
        strings: {
            type: Array,
            required: true,
            default: () => []
        },
        typeSpeed: {
            type: Number,
            default: 50
        },
        backSpeed: {
            type: Number,
            default: 50
        },
        loop: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            displayedText: '',
            currentStringIndex: 0,
            currentCharIndex: 0,
            isDeleting: false
        };
    },
    mounted() {
        this.type();
    },
    methods: {
        type() {
            const currentString = this.strings[this.currentStringIndex] || '';
            if (this.isDeleting) {
                this.displayedText = currentString.substring(0, this.currentCharIndex - 1);
                this.currentCharIndex--;
            } else {
                this.displayedText = currentString.substring(0, this.currentCharIndex + 1);
                this.currentCharIndex++;
            }

            let typeSpeed = this.typeSpeed;
            if (this.isDeleting) {
                typeSpeed = this.backSpeed;
            }

            if (!this.isDeleting && this.currentCharIndex === currentString.length) {
                if (this.loop) {
                    this.isDeleting = true;
                    typeSpeed = 1000; // Pause before deleting
                }
            } else if (this.isDeleting && this.currentCharIndex === 0) {
                this.isDeleting = false;
                this.currentStringIndex = (this.currentStringIndex + 1) % this.strings.length;
                typeSpeed = 500; // Pause before typing next string
            }

            setTimeout(this.type, typeSpeed);
        }
    }
};
</script>
