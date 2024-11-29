<template>
    <header class="bg-white py-6 drop-shadow">
        <div class="container mx-auto flex justify-between items-center px-4">
            <div>
                <h1 class="text-2xl font-bold text-orange-600 m-8">
                    David Acosta Laverde
                </h1>
            </div>
            <nav>
                <ul class="flex space-x-4">
                    <li>
                        <a
                            href="#about"
                            class="text-gray-900 hover:text-orange-600"
                            >About</a
                        >
                    </li>
                    <li>
                        <a
                            href="#projects"
                            class="text-gray-900 hover:text-orange-600"
                            >Projects</a
                        >
                    </li>
                    <li>
                        <a
                            href="#contact"
                            class="text-gray-900 hover:text-orange-600"
                            >Contact</a
                        >
                    </li>
                </ul>
            </nav>
        </div>
    </header>
</template>
<script>
export default { name: "Header" };
</script>
