<template>
    <form @submit.prevent="add">
        <div id="titleText">
            <h3> 포토갤러리 사진 등록</h3>
        </div>
        <div class="form-group">
            <label for="galleryTitle" class="form-label mt-4">사진 제목</label>
            <input type="text" class="form-control" id="galleryTitle" ref="input_title" v-model.lazy="gallery.title" placeholder="사진에 대한 짧은 글을 작성해주세요" maxlength="30">
            <label for="galleryArea" class="form-label mt-4">사진 장소</label>
            <input type="text" class="form-control" id="galleryArea" ref="input_area" v-model.lazy="gallery.area" placeholder="사진 속 장소를 입력해주세요" maxlength="30">
        </div>
        <div class="form-group formFile_image">
            <label for="formFile" class="form-label mt-4">사진 업로드</label>
            <input class="form-control" type="file" id="formFile" @change="change">
            <div class="preview">
                <img :src="imageUrl" id="photo_preview">
            </div>
        </div>
        <div class="submit-btn">
            <button type="submit" class="btn btn-primary">등록</button>
        </div>
    </form>
</template>

<script>
import {ref, watch} from "vue";
import axios from '@/setting/axiossetting.js';
import {useRouter} from 'vue-router';
export default {
    props: {
        parent_id: {
            type: String,
            required: true
        }
    },
    emits: ['parent_getSession'],
    setup(props, context) {
        context.emit("parent_getSession");
        const gallery = ref({
            user:'',
            title:'',
            area:'',
            fileName:''
        })
        let imageFile = ref("");
        let imageUrl = ref("");
        const router = useRouter();

        const change = event => {
            if (event.target.files.length === 0) {
                return;
            }
            imageFile.value = event.target.files[0];
        }

        // 이미지 미리보기
        watch(imageFile, (imageFile) => {
            let fileReader = new FileReader();
            fileReader.readAsDataURL(imageFile);

            fileReader.addEventListener("load", () =>{
                imageUrl.value = fileReader.result;
            })
        })

        const add = async() => {
            let frm = new FormData();
            if (!gallery.value.title) {
                alert("사진 제목을 입력해주세요");
                //this.$refs.input_title.focus();
                return false;
            }
            if (!gallery.value.area) {
                alert("사진 장소를 입력해주세요");
                return false;
            }
            if (imageFile.value != '') { // let file=''; 초기 값 상태가 아닌지 확인. 즉 파일을 선택하면 파일 업로드하기
                frm.append("uploadfile", imageFile.value);
            } else {
                alert("사진 업로드는 필수입니다");
                return false;
            }
            frm.append("title", gallery.value.title);
            frm.append("area", gallery.value.area);
            frm.append("user_id", props.parent_id);

            try {
                const res = await axios.post('gallery/new',
                    frm,
                    { header: 
                        {'Content-Type':'multipart/form-data;charset=UTF-8'}
                    },
                )
                console.log(res.data);
                
                router.push({
                    name: 'GalleryMain'
                })
            } catch(err) {
                console.log('여기는 오류')
                console.log(err)
            }
        }
        return {
            gallery, change, add, imageUrl
        };
    }

}
</script>

<style scoped>
    form {
        margin: 40px 200px 20px 200px;
    }

    #titleText {
        text-align: center;
    }

    h3 {
        font-weight: 600;
    }

    .form-group {
        width: 750px;
        margin: 0 auto;
    }

    .formFile_image {
        margin-bottom: 20px;
    }

    #formFile {
        margin-bottom: 20px;
    }

    .preview {
        margin: 0 auto;
        border-style: dotted;
        border-width: 0.5px;
        border-radius: 2%;
        width: 750px;
        height: 422px;
        overflow: hidden;      
        text-align: center;
    }

    #photo_preview {
        width: auto;
        height: 100%;
    }

    .submit-btn {
        text-align: center;
    }

</style>