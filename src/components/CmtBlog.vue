<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-sm-4 mb-4 m-auto" v-for="(item, index) in items" :key="index">
                <div class="card h-100 shadow-lg bg-body rounded">
                    <img :src="item.image" class="card-img-top" alt="Hình ảnh" srcset=""> 
                    <div class="card-body d-flex flex-column"> 
                        <h3 class="card-title">{{ item.title }}</h3>
                        <p class="card-text flex-grow-1">{{ item.content }}</p>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Phần bình luận -->
        <div class="row mt-5">
            <div class="col-md-8 mx-auto">
                <div class="comment-section">
                    <h4 class="text-primary mb-4">Bình luận bài viết</h4>
                    
                    <!-- Form nhập bình luận -->
                    <div class="comment-form mb-4">
                        <div class="mb-3">
                            <textarea 
                                v-model="newComment" 
                                class="form-control"
                                rows="3"
                                placeholder="Viết bình luận của bạn..."
                            ></textarea>
                        </div>
                        <button @click="addComment" class="btn btn-success">
                            <i class="fas fa-paper-plane me-2"></i>Gửi bình luận
                        </button>
                    </div>
                    
                    <!-- Danh sách bình luận -->
                    <div class="comments-list">
                        <div 
                            v-for="comment in comments" 
                            :key="comment.id"
                            class="comment-item mb-3"
                        >
                            <div class="d-flex align-items-start">
                                <div class="comment-avatar me-3">
                                    <i class="fas fa-user-circle fa-2x text-secondary"></i>
                                </div>
                                <div class="comment-content flex-grow-1">
                                    <div class="comment-header d-flex justify-content-between align-items-center mb-2">
                                        <strong class="comment-author text-primary">{{ comment.author }}</strong>
                                        <small class="comment-time text-muted">{{ comment.time }}</small>
                                    </div>
                                    <p class="comment-text mb-0">{{ comment.content }}</p>
                                </div>
                            </div>
                        </div>
                        
                        <div v-if="comments.length === 0" class="text-center text-muted py-4">
                            <i class="fas fa-comments fa-3x mb-3"></i>
                            <p>Chưa có bình luận nào. Hãy là người đầu tiên bình luận!</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import img1 from '../assets/Images/rau.jpg'

const items = ([
    { 
        title: '8 loại rau củ quả giàu canxi', 
        content: 'Canxi là khoáng chất rất cần thiết cho cơ thể, đặc biệt là xương và răng. Canxi cũng giúp cơ thể duy trì chức năng của các tế bào thần kinh và cơ bắp. Thiếu canxi có thể dẫn đến loãng xương, đau nhức xương khớp và các vấn đề về tim mạch.', 
        image: img1 
    },
]);

const newComment = ref('')
const comments = ref([
    {
        id: 1,
        author: 'Nguyễn Văn A',
        content: 'Bài viết rất hay và bổ ích! Cảm ơn bạn đã chia sẻ những thông tin hữu ích về canxi.',
        time: '2 giờ trước'
    },
    {
        id: 2,
        author: 'Trần Thị B', 
        content: 'Mình đã áp dụng những lời khuyên này và thấy rất hiệu quả. Cảm ơn tác giả!',
        time: '1 giờ trước'
    }
])

const addComment = () => {
    if (newComment.value.trim()) {
        const newCommentObj = {
            id: Date.now(),
            author: 'Bạn',
            content: newComment.value.trim(),
            time: 'Vừa xong'
        }
        comments.value.unshift(newCommentObj)
        newComment.value = ''
    }
}
</script>

<style scoped>
.card-img-top {
    width: 100%;
    height: 200px; 
    object-fit: cover; 
}

.comment-section {
    background-color: #f8f9fa;
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.comment-form textarea {
    border: 2px solid #e9ecef;
    border-radius: 8px;
    transition: border-color 0.3s ease;
}

.comment-form textarea:focus {
    border-color: #0d6efd;
    box-shadow: 0 0 0 0.2rem rgba(13, 110, 253, 0.25);
}

.comment-item {
    background-color: white;
    padding: 1.5rem;
    border-radius: 8px;
    border-left: 4px solid #0d6efd;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.comment-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
}

.comment-avatar {
    min-width: 40px;
}

.comment-author {
    font-size: 1rem;
    font-weight: 600;
}

.comment-text {
    color: #495057;
    line-height: 1.6;
}

.comment-time {
    font-size: 0.875rem;
}

.btn-success {
    background: linear-gradient(45deg, #28a745, #20c997);
    border: none;
    border-radius: 25px;
    padding: 0.5rem 1.5rem;
    font-weight: 500;
    transition: all 0.3s ease;
}

.btn-success:hover {
    background: linear-gradient(45deg, #218838, #1ea085);
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(40, 167, 69, 0.3);
}
</style>