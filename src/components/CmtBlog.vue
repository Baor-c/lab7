<template>
    <div class="container mt-5">
        <h2 class="text-center text-primary mb-5">Bình luận bài viết</h2>
        
        <!-- Hiển thị bài viết -->
        <div class="row justify-content-center">
            <div class="col-lg-8 col-md-10">
                <div v-for="(item, index) in items" :key="index" class="mb-5">
                    <div class="card shadow-lg border-0">
                        <img :src="item.image" class="card-img-top" alt="Hình ảnh bài viết" style="height: 300px; object-fit: cover;">
                        <div class="card-body p-4">
                            <h3 class="card-title text-primary mb-3">{{ item.title }}</h3>
                            <p class="card-text text-muted">{{ item.content }}</p>
                        </div>
                    </div>

                    <!-- Form bình luận -->
                    <div class="mt-4 p-4 bg-light rounded">
                        <h5 class="mb-3">
                            <i class="fas fa-comment-dots text-primary me-2"></i>
                            Để lại bình luận
                        </h5>
                        <form @submit.prevent="submitComment">
                            <div class="mb-3">
                                <label for="userName" class="form-label">Tên của bạn:</label>
                                <input 
                                    type="text" 
                                    id="userName" 
                                    class="form-control" 
                                    v-model="userName"
                                    placeholder="Nhập tên của bạn"
                                    required
                                >
                            </div>
                            <div class="mb-3">
                                <label for="commentText" class="form-label">Bình luận:</label>
                                <textarea 
                                    id="commentText" 
                                    class="form-control" 
                                    rows="4"
                                    v-model="commentText"
                                    placeholder="Nhập bình luận của bạn..."
                                    required
                                ></textarea>
                            </div>
                            <button type="submit" class="btn btn-primary">
                                <i class="fas fa-paper-plane me-2"></i>
                                Gửi bình luận
                            </button>
                        </form>
                    </div>

                    <!-- Hiển thị danh sách bình luận -->
                    <div v-if="comments.length > 0" class="mt-4">
                        <h5 class="mb-3">
                            <i class="fas fa-comments text-primary me-2"></i>
                            Bình luận ({{ comments.length }})
                        </h5>
                        <div class="comments-list">
                            <div 
                                v-for="(comment, commentIndex) in comments" 
                                :key="commentIndex"
                                class="comment-item p-3 mb-3 bg-white rounded shadow-sm border-start border-primary border-4"
                            >
                                <div class="d-flex justify-content-between align-items-start">
                                    <div class="flex-grow-1">
                                        <h6 class="mb-1 text-primary">
                                            <i class="fas fa-user-circle me-2"></i>
                                            {{ comment.name }}
                                        </h6>
                                        <p class="mb-2 text-dark">{{ comment.text }}</p>
                                        <small class="text-muted">
                                            <i class="fas fa-clock me-1"></i>
                                            {{ comment.time }}
                                        </small>
                                    </div>
                                    <button 
                                        @click="deleteComment(commentIndex)"
                                        class="btn btn-outline-danger btn-sm"
                                        title="Xóa bình luận"
                                    >
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Thông báo khi chưa có bình luận -->
                    <div v-else class="mt-4 text-center text-muted">
                        <i class="fas fa-comment-slash fa-2x mb-2"></i>
                        <p>Chưa có bình luận nào. Hãy là người đầu tiên bình luận!</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
import img1 from '../assets/Images/rau.jpg'

// Dữ liệu bài viết
const items = [
    { 
        title: '8 loại rau củ quả giàu canxi', 
        content: 'Canxi là khoáng chất rất cần thiết cho cơ thể, đặc biệt là xương và răng. Canxi cũng giúp cơ thể duy trì chức năng của các tế bào thần kinh và cơ bắp. Thiếu canxi có thể dẫn đến loãng xương, đau nhức xương khớp và các vấn đề về tim mạch. Các loại rau củ quả giàu canxi bao gồm: cải xanh, súp lơ xanh, cải thảo, rau muống, rau dền, bí đỏ, cà rốt và khoai lang. Việc bổ sung những thực phẩm này vào chế độ ăn hàng ngày sẽ giúp cung cấp đủ lượng canxi cần thiết cho cơ thể.', 
        image: img1 
    }
]

// Các biến reactive cho form bình luận
const userName = ref('')
const commentText = ref('')
const comments = ref([])

// Hàm định dạng thời gian
const formatTime = () => {
    const now = new Date()
    return now.toLocaleString('vi-VN', {
        year: 'numeric',
        month: '2-digit',
        day: '2-digit',
        hour: '2-digit',
        minute: '2-digit'
    })
}

// Hàm xử lý gửi bình luận
const submitComment = () => {
    if (userName.value.trim() && commentText.value.trim()) {
        const newComment = {
            name: userName.value.trim(),
            text: commentText.value.trim(),
            time: formatTime()
        }
        
        comments.value.unshift(newComment) // Thêm bình luận mới lên đầu
        
        // Reset form
        userName.value = ''
        commentText.value = ''
        
        // Hiển thị thông báo thành công
        alert('Bình luận đã được gửi thành công!')
    }
}

// Hàm xóa bình luận
const deleteComment = (index) => {
    if (confirm('Bạn có chắc chắn muốn xóa bình luận này?')) {
        comments.value.splice(index, 1)
    }
}
</script>

<style scoped>
.card {
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15) !important;
}

.comment-item {
    transition: all 0.3s ease;
}

.comment-item:hover {
    transform: translateX(5px);
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.btn {
    transition: all 0.3s ease;
}

.btn:hover {
    transform: translateY(-2px);
}

.form-control:focus {
    border-color: var(--bs-primary);
    box-shadow: 0 0 0 0.2rem rgba(13, 110, 253, 0.25);
}

.comments-list {
    max-height: 500px;
    overflow-y: auto;
}

.comments-list::-webkit-scrollbar {
    width: 6px;
}

.comments-list::-webkit-scrollbar-track {
    background: #f1f1f1;
    border-radius: 3px;
}

.comments-list::-webkit-scrollbar-thumb {
    background: var(--bs-primary);
    border-radius: 3px;
}

.comments-list::-webkit-scrollbar-thumb:hover {
    background: #0056b3;
}

.border-start {
    border-left-width: 4px !important;
}

@media (max-width: 768px) {
    .container {
        padding: 0 15px;
    }
    
    .card-body {
        padding: 1.5rem !important;
    }
    
    .comment-item {
        padding: 1rem !important;
    }
}
</style>