<script setup>
    import {ref, reactive} from "vue";
    import { router, usePage} from "@inertiajs/vue3";
    import AlertCircleOutlineIcon from 'vue-material-design-icons/AlertCircleOutline.vue';
    import CellphoneLinkIcon from 'vue-material-design-icons/CellphoneLink.vue';
    import ClockEditOutlineIcon from 'vue-material-design-icons/ClockEditOutline.vue';
    import CloseThickIcon from 'vue-material-design-icons/CloseThick.vue';
    import MapMarkerOutline from "vue-material-design-icons/MapMarkerOutline.vue";
    import flatPickr from "vue-flatpickr-component";
    import "flatpickr/dist/flatpickr.css";
    import "../../css/flatpickrDark.css";

    // const uer = usePage().props.auth.user;
    const emit = defineEmits(["close"]);
    const form = reactive({ text: null, file: null, duration: "", loc: "" });
    const flatPickrConfig = ref({
        wrap: true,
        dateForm:"d-m-Y",
        maxDate: new Date(new Date().setMonth(new Date().getMonth() + 3)),
        minDate: new Date(new Date().setDate(new Date().getDate() + 1)),
    });
    let isValidFile = ref(null);
    let fileDisplay = ref("");
    let comment = ref("");
    let error = ref({text: null, file: null});

    function close()
    {
        form.text = null;
        form.file = null;
        form.duration = ""
        form.loc = ""
        fileDisplay.value = '';
        emit('close');
    }

    function getUploadedImage(e)
    {
        form.file = e.target.files[0];
        let ext = form.file.name.substring(form.file.name.lastIndexOf('.') + 1);

        if (ext === 'png' || ext === 'jpg' || ext === 'jpeg' || ext === 'webp')
        {
            isValidFile.value = true;
        }
        else
        {
            isValidFile.value = false;
            return;
        }

        fileDisplay.value = URL.createObjectURL(form.file);
        setTimeout(_ =>
        {
            // document.querySelector("#postComment").scrollIntoView({behavior: "smooth"});
        }, 300);
    }

    function addPost()
    {
        error.value.text = null;
        error.value.file = null;
        if (form.text === null || form.text === '')
        {
            error.value.text = "Please add a comment";
            return;
        }

        if (form.file === null)
        {
            error.value.file = "Please add an image";
            return;
        }

        router.post("/post", form, {
            forceFormData: true,
            preserveScroll: true,
            onError: errors =>
            {
                errors && errors.text ? error.value.text = errors.text : "";
                errors && errors.file ? error.value.file = errors.file : "";
            },
            onSuccess: () => close()
        });
    }

</script>

<template>
    <div id="AddPost" class="modal addPost">

        <div class="modalContent">
            <div class="header">
                <CloseThickIcon class="icon altBtnHover" :size="35" fill-color="#737373" @click="close()"/>
                <h2>New Post</h2>
                <button class="btn btnPrimary">Share</button>
            </div>
            <div class="body">
                <div class="bodyContainer">
                    <div class="commentText">
                        <div class="profileContainer">
                            <div class="profile">
                                <img src="https://picsum.photos/id/64/300/320" alt="Profile Image">
                                <h3>Some Name</h3>
                            </div>
                        </div>
                        <div class="error" v-if="error && error.text">{{error.text}}</div>
                        <div class="commentTextContainer">
                            <textarea ref="comment" v-model="form.text" placeholder="What's on your mind?"
                                      name="comment" id="comment" rows="10">
                            </textarea>
                        </div>
                        <div class="formContainer">
                            <label for="loc">Add Location <MapMarkerOutline class="icon" :size="35"/></label>
                            <input type="text" name="loc" id="loc" placeholder="Location">
                        </div>
                        <div class="formContainer">
                            <label for="duration">Duration <ClockEditOutlineIcon class="icon" :size="35"/></label>
                            <flat-pickr name="duration" id="duration" v-model="form.duration" placeholder="Duration"
                                        :config="flatPickrConfig"/>
                        </div>
                    </div>
                    <div class="addFile">
                        <div class="addFileInput">
                            <label for="file" class="altBtnHover"><CellphoneLinkIcon class="icon" :size="35"/></label>
                            <input type="file" id="file" class="hidden" @input="$event => getUploadedImage($event)">
                            <div v-if="error && error.file" class="error">
                                <AlertCircleOutlineIcon class="icon" :size="35"/> {{ error.file }}
                            </div>
                            <div v-if="!fileDisplay && isValidFile === false" class="error">
                                <AlertCircleOutlineIcon class="icon" :size="35"/> File not accepted
                            </div>
                        </div>
                        <img :src="fileDisplay" :alt="fileDisplay ? 'Selected Image' : ''" class="showImage" v-if="fileDisplay">
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
