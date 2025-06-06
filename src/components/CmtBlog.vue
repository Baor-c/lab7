<template>
    <div class="container mt-5 text-center ">
        <h3 class="text-3xl font-bold mb-4 text-primary">Bình luận bài viết</h3>
        <div class="row">
            <div class="col-md-5 mx-auto " v-for="(item, index) in items" :key="index">
                <div class="card shadow mb-4">
                    <img :src="item.image" class="card-img-top" alt="Hình ảnh" srcset="">
                    <div class="card-body d-flex flex-column">
                        <h3 class="card-title">{{ item.title }}</h3>
                        <p class="card-text flex-grow-1">{{ item.content }}</p>
                    </div>
                </div>
                <form @submit.prevent="submitComment">
                    <div class="mt-3">
                        <textarea id="commentText" cols="60" v-model="commentText"
                            placeholder="Nhập bình luận của bạn"></textarea>
                    </div>
                    <button type="submit" class="btn btn-success">Gửi bình luận</button>
                </form>

                <div v-if="comments.length" class="mt-3">
                    <h5>Danh sách các bình luận:</h5>
                    <ul style="list-style-type: circle;">
                        <li v-for="(comment, index) in comments" :key="index">
                            <p><strong>{{ comment.name }}:</strong> {{ comment.text }}</p>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import { ref } from 'vue'
import img1 from '../assets/Images/rau.jpg'

const items = ([
    { title: '8 loại rau củ quả giàu canxi', content: 'Canxi là khoáng chất rất cần thiết cho cơ thể, đặc biệt là xương và răng. Canxi cũng giúp cơ thể duy trì chức năng của các tế bào thần kinh và cơ bắp. Thiếu canxi có thể dẫn đến loãng xương, đau nhức xương khớp và các vấn đề về tim mạch.', image: img1 },
]);

// Khai báo các biến reactive
const commentText = ref('')
const comments = ref([])

// Hàm xử lý gửi bình luận
const submitComment = () => {
    if (commentText.value.trim()) {
        comments.value.push({
            name: 'Người dùng',
            text: commentText.value.trim()
        })
        commentText.value = '' // Xóa nội dung sau khi gửi
    }
}
</script>

<style scoped>
.card-img-top {
    width: 100%;
    object-fit: cover;
}
</style>