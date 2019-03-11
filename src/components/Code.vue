<template>
    <pre class="language-c line-numbers">
        <code>{{sourceCodeParallel}}</code>
    </pre>
</template>

<script>
    import 'prismjs'
    import 'prismjs/components/prism-c'
    import 'prismjs/plugins/line-numbers/prism-line-numbers.css'
    import 'prismjs/plugins/line-numbers/prism-line-numbers'
    import 'prismjs/themes/prism.css'
    export default {
        props: {
            parallel: {type: Boolean}
        },
        name: "Code",
        data() {
            return {
                sourceCodeParallel: `
        int v1[6]= {3,4,5,6,7,8};
        int v2[6]= {4,5,6,7,8,9};
        int v3[6];
        int B_THREAD = getNBTHREAD();

        #pragma OMP parallel
        {
            int TID = getTID();

            int s = TID;
            while(s < v1.length)
            {
                v3[s] = v1[s] * v2[s];
                s += NB_THREAD;
            }
        }
        `,
                sourceCode: `
        int v1[6]= {3,4,5,6,7,8};
        int v2[6]= {4,5,6,7,8,9};
        int v3[6];
        int B_THREAD = getNBTHREAD();

        int TID = getTID();

        int s = TID;
        while(s < v1.length)
        {
            v3[s] = v1[s] * v2[s];
            s += NB_THREAD;
        }



           `
            }
        }
    }
</script>

<style scoped>

</style>
